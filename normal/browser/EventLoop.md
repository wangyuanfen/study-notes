Event Loop即事件循环，是指浏览器或Node的一种解决javaScript单线程运行时不会阻塞的一种机制，也就是我们经常使用异步的原理。

## 浏览器渲染流程
```
- 浏览器输入url，浏览器主进程接管，开一个下载线程，
然后进行 http请求（略去DNS查询，IP寻址等等操作），然后等待响应，获取内容，
随后将内容通过RendererHost接口转交给Renderer进程

- 浏览器渲染流程开始
```
浏览器器内核拿到内容后，渲染大概可以划分成以下几个步骤：
1. 解析html建立dom树
2. 解析css构建render树（将CSS代码解析成树形的数据结构，然后结合DOM合并成render树）
3. 布局render树（Layout/reflow），负责各元素尺寸、位置的计算
4. 绘制render树（paint），绘制页面像素信息
5. 浏览器会将各层的信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上。

所有详细步骤都已经略去，渲染完毕后就是load事件了，之后就是自己的JS逻辑处理了

## 浏览器内核（渲染进程)
1. GUI渲染线程
* 负责渲染浏览器界面，解析HTML，CSS，构建DOM树和RenderObject树，布局和绘制等。
* 当界面需要重绘（Repaint）或由于某种操作引发回流(reflow)时，该线程就会执行
* 注意，GUI渲染线程与JS引擎线程是互斥的，当JS引擎执行时GUI线程会被挂起（相当于被冻结了），GUI更新会被保存在一个队列中等到JS引擎空闲时立即被执行。
2. JS引擎线程
* 也称为JS内核，负责处理Javascript脚本程序。（例如V8引擎）
* JS引擎线程负责解析Javascript脚本，运行代码。
* JS引擎一直等待着任务队列中任务的到来，然后加以处理，一个Tab页（renderer进程）中无论什么时候都只有一个JS线程在运行JS程序
* 同样注意，GUI渲染线程与JS引擎线程是互斥的，所以如果JS执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞。
3. 事件触发线程
* 归属于浏览器而不是JS引擎，用来控制事件循环（可以理解，JS引擎自己都忙不过来，需要浏览器另开线程协助）
* 当JS引擎执行代码块如setTimeOut时（也可来自浏览器内核的其他线程,如鼠标点击、AJAX异步请求等），会将对应任务添加到事件线程中
* 当对应的事件符合触发条件被触发时，该线程会把事件添加到待处理队列的队尾，等待JS引擎的处理
* 注意，由于JS的单线程关系，所以这些待处理队列中的事件都得排队等待JS引擎处理（当JS引擎空闲时才会去执行）
4. 定时触发器线程
* 传说中的setInterval与setTimeout所在线程
* 浏览器定时计数器并不是由JavaScript引擎计数的,（因为JavaScript引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确）
* 因此通过单独线程来计时并触发定时（计时完毕后，添加到事件队列中，等待JS引擎空闲后执行）
* 注意，W3C在HTML标准中规定，规定要求setTimeout中低于4ms的时间间隔算为4ms。
5. 异步http请求线程
* 在XMLHttpRequest在连接后是通过浏览器新开一个线程请求
* 将检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件，将这个回调再放入事件队列中。再由JavaScript引擎执行。

## JS分为同步任务和异步任务
1. 同步任务都在主线程上执行，形成一个执行栈
2. 主线程之外，事件触发线程管理着一个任务队列，只要异步任务有了运行结果，就在任务队列之中放置一个事件。
3. 一旦执行栈中的所有同步任务执行完毕（此时JS引擎空闲），系统就会读取任务队列，将可运行的异步任务添加到可执行栈中，开始执行。
4. 总是要等待栈中的代码执行完毕后才会去读取任务队列中的事件
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563463752828.jpg?raw=true)

## setTimeout
```
setTimeout(function(){
    console.log('hello!');
}, 1000);
```
当1000毫秒计时完毕后（由定时器线程计时），将回调函数推入事件队列中，等待主线程执行
```
setTimeout(function(){
    console.log('hello!');
}, 0);

console.log('begin');

// begin
// hello
```
0毫秒后就推入事件队列，但是W3C在HTML标准中规定，规定要求setTimeout中低于4ms的时间间隔算为4ms。假设0毫秒就推入事件队列，也会先执行begin（因为只有可执行栈内空了后才会主动读取事件队列）

## Macrotask(宏任务)与Microtask(微任务)
JS中分为两种任务类型：macrotask和microtask，在ECMAScript中，microtask称为jobs，macrotask可称为task
1. macrotask（又称之为宏任务），可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）
* 每一个task会从头到尾将这个任务执行完毕，不会执行其它
* 浏览器为了能够使得JS内部task与DOM任务能够有序的执行，会在一个task执行结束后，在下一个 task 执行开始前，对页面进行重新渲染
（task->渲染->task->...）
* 主代码块，setTimeout，setInterval等（可以看到，事件队列中的每一个事件都是一个macrotask）
2. microtask（又称为微任务），可以理解是在当前 task 执行结束后立即执行的任务
* 也就是说，在当前task任务后，下一个task之前，在渲染之前
* 所以它的响应速度相比setTimeout（setTimeout是task）会更快，因为无需等渲染
* 也就是说，在某一个macrotask执行完后，就会将在它执行期间产生的所有microtask都执行完毕（在渲染前）
* Promise，process.nextTick等

注意：在node环境下，process.nextTick的优先级高于Promise，也就是可以简单理解为：在宏任务结束后会先执行微任务队列中的nextTickQueue部分，然后才会执行微任务中的Promise部分。

总结下运行机制：
1. 执行一个宏任务（栈中没有就从事件队列中获取）
2. 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
3. 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
4. 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
5. 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）
![](https://github.com/wangyuanfen/study-notes/blob/master/image/1563464666560.jpg?raw=true)
例子
```
console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
    console.log('promise1');
}).then(function() {
    console.log('promise2');
});

console.log('script end');

\\ script start
\\ script end
\\ promise1
\\ promise2
\\ setTimeout
```
