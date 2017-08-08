### 在 XML 中使用 Bitmap

	<?xml version ="1.0" encoding="utf-8"?>
	<bitmap xmlns:android="http://schemas.android.com/apk/res/android"
			android:src="@drawable/tab_a"/>

通过这样引用图片，就可以将图片直接转换成 Bitmap 了。

### Shape

	<?xml version="1.0" encoding="utf-8"?>
	<shape xmlns:android="http://schemas.android.com/apk/res/android"android:shape="rectangle" >

	<!--设置弧角半径，当然也可以单独设置每个角的弧角半径，如下：-->
    <corners
        android:topLeftRadius="0dp"
        android:topRightRadius="0dp"
        android:bottomLeftRadius="0dp"
        android:bottomRightRadius="0dp"/>

	<!-- 单独设置弧角半径 -->
	<corners android:radius="14dp"/>

	<!--这里是设置填充色的，即背景色-->
	<solid android:color="#4F4F4F"/>

	<!--设置内间距-->
	<padding
		android:left="0dp"
		android:top="0dp"
		android:right="0dp"
		android:bottom="0dp"/>

	<!--这个都知道吧，设置控件的尺寸的，最好和Button一致-->
	<size
		android:width="270dp"
		android:height="60dp"/>

	<!--这是是设置外边框的。width是宽度，color是外边框颜色 -->
	<stroke
		android:width="3dp"
		android:color="#faaaaa"/>

	<!-- 渐变色的 
		angle-->渐变角度(PS：当angle=0时，渐变色是从左向右。 然后逆时针方向转，当angle=90时为从下往上。angle必须为45的整数倍)

		centerX-->渐变中心X点坐标的相对位置

		centerColor-->从起始颜色到终点颜色的中间颜色

		startColor--> 起始颜色

		endColor--> 终点颜色

		type-->渐变类型
    		linear 线性渐变，这是默认设置
    		radial 放射性渐变，以开始色为中心。
    		sweep 扫描线式的渐变。 -->

	<gradient
		android:angle="45"
		android:centerX="35%"
		android:centerColor="#7995A8"
		android:startColor="#E8E8E8"
		android:endColor="#000000"
		android:type="linear"/>

	</shape>


### Layer

将多个图片或上面两种效果按照顺序层叠起来

	<?xml version="1.0" encoding="utf-8"?>  
	<layer-list xmlns:android="http://schemas.android.com/apk/res/android"> 
 
    <item>  
      <bitmap android:src="@drawable/android_red"  
        android:gravity="center" />  
    </item>  

    <item android:top="10dp" android:left="10dp">  
      <bitmap android:src="@drawable/android_green"  
        android:gravity="center" />  
    </item>  

    <item android:top="20dp" android:left="20dp">  
      <bitmap android:src="@drawable/android_blue"  
        android:gravity="center" />  
    </item>  

	</layer-list> 

 ![Layer](http://i.imgur.com/aQSxazU.png)


### Selector

根据不同的选定状态来定义不同的现实效果
分为四大属性：

	android:state_selected 是否选中
	android:state_focused  是否获得焦点
	android:state_pressed  是否按压
	android:state_enabled  是否设置是否响应事件,指所有事件
PS：

	android:state_window_focused 默认时的背景图片

引用位置：res/drawable/ 文件的名称 .xml

	<?xml version="1.0" encoding="utf-8"?>
	<selector xmlns:android="http://schemas.android.com/apk/res/android">

    	<item android:drawable="@drawable/shape_btn_city_hot_clicked" 	android:state_pressed="true" /> <!-- pressed -->

    	<item android:drawable="@drawable/shape_btn_city_hot_clicked" 	android:state_focused="true" /> <!-- focused -->

		<item android:drawable="@drawable/shape_btn_city_hot_unclick" />  <!-- default -->

	</selector>

详细介绍：

	<?xml version="1.0" encoding="utf-8" ?>       
	<selector xmlns:Android="http://schemas.android.com/apk/res/android">     
		<!-- 默认时的背景图片-->      
		<item Android:drawable="@drawable/pic1" /> 
       
		<!-- 没有焦点时的背景图片 -->      
		<item   
   			Android:state_window_focused="false"        
   			android:drawable="@drawable/pic_blue"   /> 
      
		<!-- 非触摸模式下获得焦点并单击时的背景图片 -->      
		<item   
   			Android:state_focused="true"   
   			android:state_pressed="true"     
   			android:drawable= "@drawable/pic_red"   />
     
		<!-- 触摸模式下单击时的背景图片-->      
		<item   
   			Android:state_focused="false"   
   			Android:state_pressed="true"     
   			Android:drawable="@drawable/pic_pink"   /> 
     
		<!--选中时的图片背景-->      
		<item   
   			Android:state_selected="true"   
   			android:drawable="@drawable/pic_orange"   /> 
      
		<!--获得焦点时的图片背景-->      
		<item   
   			Android:state_focused="true"   
   			Android:drawable="@drawable/pic_green"   /> 
      
		</selector>  


