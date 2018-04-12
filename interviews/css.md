### 盒子模型  
CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。    
不同部分的说明：
+ Margin(外边距) - 清除边框外的区域，外边距是透明的。
+ Border(边框) - 围绕在内边距和内容外的边框。
+ Padding(内边距) - 清除内容周围的区域，内边距是透明的。
+ Content(内容) - 盒子的内容，显示文本和图像。

### css优先级
important声明 1,0,0,0
ID选择器 0,1,0,0
类选择器 0,0,1,0
伪类选择器 0,0,1,0
属性选择器 0,0,1,0
标签选择器 0,0,0,1
伪元素选择器 0,0,0,1
通配符选择器 0,0,0,0

### BFC
具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。  
只要元素满足下面任一条件即可触发 BFC 特性：

+ body 根元素
+ 浮动元素：float 除 none 以外的值
+ 绝对定位元素：position (absolute、fixed)
+ display 为 inline-block、table-cells、flex
+ overflow 除了 visible 以外的值 (hidden、auto、scroll)
应用场景:左列布局不确定，右列自适应；

      <div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
      <div style="width: 200px; height: 200px;background: #eee;overflow: hidden;">我是一个没有设置浮动, 触发 BFC 元素, width: 
      200px; height:200px; background: #eee;</div>
      
      http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html
