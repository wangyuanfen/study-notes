### var const let
ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效  
var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined  
let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错  
let不允许在相同作用域内，重复声明同一个变量。  

const声明一个只读的常量。一旦声明，常量的值就不能改变,只声明不赋值，就会报错.  
const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

### 数组去重
es6 set,Array.from

    var arr = [1,1,'1','1',null,null,undefined,undefined,NaN,NaN];
    var newArr = Array.from(new Set(arr));
    console.log(newArr);//[1, "1", null, undefined, NaN]
    
Chrome,Firfox,Opera，Safari，包括微软的Edge,都是支持的，唯独IE系列不支持。

### ES6常用知识点
