## Set
ES6 提供了新的数据结构 Set。

它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set本身是一个构造函数，用来生成 Set 数据结构。
```
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```
**Set 实例的属性和方法**
**属性**  
Set.prototype.constructor：构造函数，默认就是Set函数。
Set.prototype.size：返回Set实例的成员总数。

**操作方法**  
1. Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
2. Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
3. Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
4. Set.prototype.clear()：清除所有成员，没有返回值
```
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```
Array.from方法可以将 Set 结构转为数组。
```
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```
**遍历方法**  
Set.prototype.keys()：返回键名的遍历器
Set.prototype.values()：返回键值的遍历器
Set.prototype.entries()：返回键值对的遍历器
Set.prototype.forEach()：使用回调函数遍历每个成员
```
let set = new Set(['a', 'b', 'c']);
console.log(set.keys()); // SetIterator {"a", "b", "c"}
console.log([...set.keys()]); // ["a", "b", "c"]

let set = new Set(['a', 'b', 'c']);
console.log(set.values()); // SetIterator {"a", "b", "c"}
console.log([...set.values()]); // ["a", "b", "c"]

let set = new Set(['a', 'b', 'c']);
console.log(set.entries()); // SetIterator {"a", "b", "c"}
console.log([...set.entries()]); // [["a", "a"], ["b", "b"], ["c", "c"]]

let set = new Set([1, 2, 3]);
set.forEach((value, key) => console.log(key + ': ' + value));
// 1: 1
// 2: 2
// 3: 3
```
## Map
ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
```
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```
Map构造函数接受数组作为参数
```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```
同一个键多次赋值，后面的值将覆盖前面的值
```
const map = new Map();

map
.set(1, 'aaa')
.set(1, 'bbb');

map.get(1) // "bbb"
```
读取一个未知的键，则返回undefined
```
new Map().get('asfddfsasadf') // undefined
```
**Map 实例的属性和方法**
**属性**  
size属性返回 Map 结构的成员总数。
```
const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
```
**操作方法**
1. Map.prototype.set(key, value) // set方法设置键名key对应的键值为value，然后返回整个 Map 结构
2. Map.prototype.get(key) // get方法读取key对应的键值，如果找不到key，返回undefined。
3. Map.prototype.has(key) // has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
4. Map.prototype.delete(key) // delete方法删除某个键，返回true。如果删除失败，返回false。
5. Map.prototype.clear() // clear方法清除所有成员，没有返回值。
```
// set方法返回的是当前的Map对象，因此可以采用链式写法
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

m.get(1) // a
m.has(1) // true
m.delete(1)
m.has(1) // false
m.size // 3
m.clear()
m.size // 0
```
**遍历方法**
1. Map.prototype.keys()：返回键名的遍历器。
2. Map.prototype.values()：返回键值的遍历器。
3. Map.prototype.entries()：返回所有成员的遍历器。
4. Map.prototype.forEach()：遍历 Map 的所有成员。
