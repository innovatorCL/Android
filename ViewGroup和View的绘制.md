### ViewGroup和View的绘制过程

1. View的绘制过程包括`onMeasure()`，`onLayout()`，`onDraw()`。父View的measure的过程会先测量子View，等子View测量结果出来后，再来测量自己


2. 