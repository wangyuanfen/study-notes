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

## 引用
* [深入理解 new 操作符](https://www.cnblogs.com/onepixel/p/5043523.html)
