# 自定义ViewGroup

- 自定义ViewGroup通常需要重写`onMeasure()`方法来对子View进行测量，重写`onLayout()`来确定子View的位置，重写`onTouchEvent()`方法增加响应事件。

## 举个栗子
- 代码如下：

public class ScrollViewler extends ViewGroup{

  private Scroller mScroller;
  private int mScreenHeight;   //屏幕高度
  private int mLastY;   //上次触摸的y位置
  private int mStart;  //触摸的起点
  private int mEnd; //触摸的终点

  public ScrollViewler(Context context) {
    super(context);
    initView(context);
  }

  public ScrollViewler(Context context, AttributeSet attrs) {
    super(context, attrs);
    initView(context);
  }

  public ScrollViewler(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    initView(context);
  }

  private void initView(Context context){
    WindowManager manager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
    DisplayMetrics displayMetrics = new DisplayMetrics();
    manager.getDefaultDisplay().getMetrics(displayMetrics);
    mScreenHeight = displayMetrics.heightPixels;   //获取屏幕高度
    mScroller = new Scroller(context);  //初始化Scroller
  }


  @Override
  protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    int childCount = getChildCount();
    for (int i=0;i<childCount;i++){   //遍历测量子View的宽高
      View child = getChildAt(i);
      measureChild(child,widthMeasureSpec,heightMeasureSpec);
    }
  }


  @Override
  protected void onLayout(boolean changed, int l, int t, int r, int b) {
    //先设置好ViewGroup的高度，然后再放置子View
    int childCount = getChildCount();
    MarginLayoutParams layoutParams = (MarginLayoutParams)getLayoutParams();
    layoutParams.height = mScreenHeight *childCount;
    setLayoutParams(layoutParams);

    for (int i=0;i<childCount;i++){
      View child = getChildAt(i);
      if(child.getVisibility() != GONE){
        child.layout(l,mScreenHeight*i,r,mScreenHeight *(i+1));
      }
    }
  }


  @Override
  public boolean onTouchEvent(MotionEvent event) {
    int y = (int)event.getY();
    switch (event.getAction()){
      case MotionEvent.ACTION_DOWN:
        mLastY = y;
        mStart = getScrollY();   //记录触摸起点
        break;
      case MotionEvent.ACTION_MOVE:
        if(!mScroller.isFinished()){
          mScroller.abortAnimation();  //设置滚动动画
        }
        int dy = mLastY - y;
        if(getScrollY() <0){   //getScrollY()返回的是手机屏幕 y=0 减去 View的起始y坐标  >0代表View向上移动，<0表示向下
          dy = 0;
        }
        if(getScrollY() > getHeight() - mScreenHeight){    //滚动到了最后一张
         dy = 0;
        }
        scrollBy(0,dy);  //它表示在视图的X、Y方向上各移动dx、dy距离
                         // getScrollX() 返回的就是 dx。dx>0表示视图(View或ViewGroup)的内容从右向左滑动;反之，从左向右滑动
                         // getScrollY() 返回的就是 dy。dy>0表示视图(View或ViewGroup)的内容从下向上滑动;反之，从上向下滑动
        mLastY = y;
        break;
      case MotionEvent.ACTION_UP:   //这里实现黏性效果
//        mEnd = getScrollY();  //MOVE 事件的dy
//        int dScrollY = mEnd - mStart;   //后一次的滚动dy1 - 前一次的滚动dy0
        int dScrollY = checkAlignment();
        if(dScrollY > 0){  //视图(View或ViewGroup)的内容从下向上滑动

          if(dScrollY < mScreenHeight/3){   //黏性效果
            Log.i("TAG","向下恢复原来的图片的位置，相当于向下回弹，和上滑效果相反");
            mScroller.startScroll(0,getScrollY(),0,-dScrollY);   //第一个参数是起始移动的x坐标值，第二个是起始移动的y坐标值，
                                      // 第三个第四个参数都是移到某点的坐标值，而duration 当然就是执行移动的时间，默认完成时间250ms
          }else {
            Log.i("TAG","滑动到下一张");
            mScroller.startScroll(0,getScrollY(),0,mScreenHeight - dScrollY);
          }

        }else{  //视图(View或ViewGroup)的内容从上向下滑动

          if(-dScrollY < mScreenHeight/3){
            Log.i("TAG","向上恢复原来的图片的位置，相当于向上回弹，和下滑效果相反");
            mScroller.startScroll(0,getScrollY(),0,-dScrollY);
          }else {
            Log.i("TAG","滑动到上一张");
            mScroller.startScroll(0,getScrollY(),0,-(mScreenHeight+dScrollY));
          }
      }
        break;
    }
    postInvalidate();
    return true;  //ViewGroup已经消费，不会传递下去了
  }

  private int checkAlignment() {
    mEnd = getScrollY();   //MOVE 事件的dy
    boolean isUp = ((mEnd - mStart) > 0) ? true : false;
    int lastPrev = mEnd % mScreenHeight;  //取余，是不是ImageView的整倍数
    int lastNext = mScreenHeight - lastPrev;
    if (isUp) {
      //向上的
      return lastPrev;
    } else {
      return -lastNext;
    }
  }

  /**
   * 当我们执行ontouch或invalidate(）或postInvalidate()都会导致computeScroll()的执行
   所以当我们执行postInvalidate后，会去调computeScroll 方法，
   而这个方法里再去调postInvalidate，这样就可以不断地去调用scrollTo方法了，
   直到mScroller动画结束，当然第一次时，我们需要手动去调用一次postInvalidate才会去调用

   当startScroll执行过程中即在duration时间内，computeScrollOffset()返回true说明滚动尚未完成，false说明滚动已经完成。

   */
  @Override
  public void computeScroll() {
    super.computeScroll();
    if(mScroller.computeScrollOffset()){  //滚动未完成
      scrollTo(0,mScroller.getCurrY());
      postInvalidate();  //再次触发自身方法
    }
  }
}


