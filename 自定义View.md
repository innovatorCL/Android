# 重写View实现全新的控件

- **创建一个自定义View，难点在于绘制控件和实现交互。**

- 实现方法：一般是继承**View**,重写`onDraw()`,`onMeasure()`,涉及到嵌套的滚动的`View`的时候还会重写`onLayout()`。通过重写`onTouchEvent()`等触摸事件实现交互。






