设计师常选择iPhone6作为基准设计尺寸，交付给前端的设计尺寸是按750px * 1334px为准(高度会随着内容多少而改变)。前端开发人员通过一套适配规则自动适配到其他的尺寸。

## 视窗 viewport

简单的理解，viewport是严格等于浏览器的窗口。在桌面浏览器中，viewport就是浏览器窗口的宽度高度。

移动端的viewport太窄，为了能更好为CSS布局服务，所以提供了两个viewport:虚拟的viewportvisualviewport和布局的viewportlayoutviewport。

The visual viewport is the part of the page that’s currently shown on-screen。

The layout viewport can be considerably wider than the visual viewport, and contains elements that appear and do not appear on the screen。

## 设备像素比(device pixel ratio)
设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：
```
设备像素比 ＝ 物理像素 / 设备独立像素
```
在JavaScript中，可以通过window.devicePixelRatio获取到当前设备的dpr。而在CSS中，可以通过-webkit-device-pixel-ratio，-webkit-min-device-pixel-ratio和 -webkit-max-device-pixel-ratio进行媒体查询，对不同dpr的设备，做一些样式适配(这里只针对webkit内核的浏览器和webview)。

众所周知，iPhone6的设备宽度和高度为375pt * 667pt,可以理解为设备的独立像素；而其dpr为2，根据上面公式，我们可以很轻松得知其物理像素为750pt * 1334pt。

在不同的屏幕上，CSS像素所呈现的物理尺寸是一致的，而不同的是CSS像素所对应的物理像素具数是不一致的。在普通屏幕下1个CSS像素对应1个物理像素，而在Retina屏幕下，1个CSS像素对应的却是4个物理像素。

## CSS单位rem

```
font size of the root element.
```
rem就是相对于根元素<html>的font-size来做计算。使用rem单位，是能轻易的根据<html>的font-size计算出元素的盒模型大小。

## 使用
Flexible中，只对iOS设备进行dpr的判断，对于Android系列，始终认为其dpr为1。
```
if (!dpr && !scale) {
    var isAndroid = win.navigator.appVersion.match(/android/gi);
    var isIPhone = win.navigator.appVersion.match(/iphone/gi);
    var devicePixelRatio = win.devicePixelRatio;
    if (isIPhone) {
        // iOS下，对于2和3的屏，用2倍的方案，其余的用1倍方案
        if (devicePixelRatio >= 3 && (!dpr || dpr >= 3)) {                
            dpr = 3;
        } else if (devicePixelRatio >= 2 && (!dpr || dpr >= 2)){
            dpr = 2;
        } else {
            dpr = 1;
        }
    } else {
        // 其他设备下，仍旧使用1倍的方案
        dpr = 1;
    }
    scale = 1 / dpr;
}
```
flexible实际上就是能过JS来动态改写meta标签：
```
var metaEl = doc.createElement('meta');
var scale = isRetina ? 0.5:1;
metaEl.setAttribute('name', 'viewport');
metaEl.setAttribute('content', 'initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
if (docEl.firstElementChild) {
    document.documentElement.firstElementChild.appendChild(metaEl);
} else {
    var wrap = doc.createElement('div');
    wrap.appendChild(metaEl);
    documen.write(wrap.innerHTML);
}
```
* 动态改写<meta>标签
* 给<html>元素添加data-dpr属性，并且动态改写data-dpr的值
* 给<html>元素添加font-size属性，并且动态改写font-size的值

以750px为基础设计的, 目前Flexible会将视觉稿分成**100份**，而每一份被称为一个单位a。同时1rem单位被认定为10a。
```
1a   = 7.5px
1rem = 75px 
```
稿子就分成了10a，也就是整个宽度为10rem，<html>对应的font-size为75px

这样一来，对于视觉稿上的元素尺寸换算，只需要原始的px值除以rem基准值即可。尺寸是176px * 176px,转换成为2.346667rem * 2.346667rem。






