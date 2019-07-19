* DNS 解析
* TCP 三次握手
* 发送请求，分析 url，设置请求报文(头，主体)
* 服务器返回请求的文件 (html)
* 浏览器渲染
  * HTML parser --> DOM Tree
    * 标记化算法，进行元素状态的标记
    * dom 树构建
  * CSS parser --> Style Tree
    * 解析 css 代码，生成样式树
  * attachment --> Render Tree
    * 结合 dom树 与 style树，生成渲染树
  * layout: 布局
  * GPU painting: 像素绘制页面
