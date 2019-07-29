```
new Animal('cat') = {
    var obj = {};
    obj.__proto__ = Animal.prototype;
    var result = Animal.call(obj,"cat");
    return typeof result === 'object'? result : obj;
}
```
1. 新生成一个对象
2. 链接到原型: 把 obj 的__proto__ 指向构造函数 Animal 的原型对象 prototype，此时便建立了 obj 对象的原型链：obj->Animal.prototype->Object.prototype->null
3. 绑定this: call
4. 如果无返回值 或者 返回一个非对象值，则将 obj 作为新对象返回；否则会将 result 作为新对象返回。
```
function Foo(){
    getName = function(){
        console.log(1)
    }
    return this;
}
Foo.getName = function(){
    console.log(2)
}
Foo.prototype.getName = function(){
    console.log(3)
}
var getName = function(){
    console.log(4)
}
function getName(){
    console.log(5)
}
// ouput:
Foo.getName(); // 2
getName(); // 4
Foo().getName(); // 1; Foo() return this，指向window, 并且执行 Foo() 后, getName 进行函数赋值
getName(); // 同上
new Foo.getName(); // 2; 并且执行 new 时, Foo.getName 为构造函数
new Foo().getName(); // 3; new Foo() 生成实例，所以调取原型上的getName
new new Foo().getName(); // 3; 先执行 new Foo()，获取原型上的getName，再new 原型上的getName
```
## 引用
* [深入理解 new 操作符](https://www.cnblogs.com/onepixel/p/5043523.html)
