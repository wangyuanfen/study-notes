块级格式化上下文，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，不会在布局上影响到外面的元素，使内外元素的定位不会相互影响。

## 触发 BFC
* body 根元素
* 浮动元素：float 除 none 以外的值
* 绝对定位元素：position (absolute、fixed)
* display 为 inline-block、table-cells、flex
* overflow 除了 visible 以外的值 (hidden、auto、scroll)

## 应用
**属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠**
```
<style type="text/css">
.box {
    width: 100px;
    height: 100px;
    background: black;
    margin: 100px;
}
</style>

<body>
    <div class="box"></div>
    <div class="box"></div>
</body>
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563270749397.jpg?raw=true)

如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中
```
<style type="text/css">
.wrap {
  overflow: hidden;
}
.box {
    width: 100px;
    height: 100px;
    background: black;
    margin: 100px;
}
</style>

<body>
    <div class="wrap">
        <div class="box"></div>
    </div>
    <div class="wrap">
        <div class="box"></div>
    </div>
</body>
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563271155368.jpg?raw=true)

**可以包含浮动元素，清除内部浮动(清除浮动的原理是两个div都位于同一个 BFC 区域之中)**
```
<style type="text/css">
.box {
    padding: 10px;
    background-color: #cd0000;
}

.box>img {
    float: left;
}
</style>
<div class="box">
    <img src="test.jpg">
</div>
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563271623010.jpg?raw=true)
box形成一个BFC
```
.box {
  padding: 10px;
  background-color: #cd0000;
  overflow: hidden;
}
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563271626824.jpg?raw=true)
 
**可以阻止元素被浮动元素覆盖**
第二个元素有部分被浮动元素所覆盖
```
<body>
    <img src="test.jpg" width="100" style="float: left;">
    <div style="width: 200px; height: 200px;background: #eee;">我是一个没有设置浮动,
        也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
</body>
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563272399154.jpg?raw=true)

overflow: hidden，把第二个元素变成BFC
```
<body>
    <img src="test.jpg" width="100" style="float: left;">
    <div style="width: 200px; height: 200px;background: #eee;overflow: hidden;">我是一个没有设置浮动,
        也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;</div>
</body>
```
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563272402543.jpg?raw=true)

同时这个方法可以用来实现两列自适应布局
