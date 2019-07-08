## 定义
JavaScript 函数是被设计为执行特定任务的代码块。会在某代码调用它时被执行。

由于函数是对象，因此*函数名实际上也是一个指向函数对象的指针*，不会与某个函数绑定

创建函数的最常用的两个方法是函数表达式和函数声明。

## 函数类型

**函数声明**
首先是 function 关键字，然后是函数的名字，函数可以在全局上下文中或者函数体内声明。
```
alert(sum(10,10)); // 正常
function sum (num1, num2) { 
  return num1 + num2; 
}
```
**函数表达式**
```
alert(sum(10,10)); // unexpected identifier
var sum = function(num1, num2){ 
  return num1 + num2; 
};
```
关于函数声明，它的一个重要特征就是函数声明提升（function declaration hoisting），意思是在执行代码之前会先读取函数声明。

解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问 ），至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。

所以会出现上面两段代码的alert结果

**Function构造函数**
