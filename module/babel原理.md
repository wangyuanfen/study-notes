Babel 是 JavaScript 编译器，更确切地说是源码到源码的编译器，通常也叫做“转换编译器（transpiler）”。 意思是说你为 Babel 提供一些 JavaScript 代码，Babel 更改这些代码，然后返回给你新生成的代码。

## AST 抽象语法树
在计算机科学中，抽象语法树（abstract syntax tree 或者缩写为 AST），或者语法树（syntax tree），是源代码的抽象语法结构的树状表现形式，这里特指编程语言的源代码。树上的每个节点都表示源代码中的一种结构。。

[AST Explorer](https://astexplorer.net/)

抽象语法树其实就是将一类标签转化成通用标识符，从而结构出的一个类似于树形结构的语法树。
```
let wang = 1;
```
```
{
  "type": "Program",
  "start": 0,
  "end": 13,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 13,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 4,
          "end": 12,
          "id": {
            "type": "Identifier",
            "start": 4,
            "end": 8,
            "name": "wang"
          },
          "init": {
            "type": "Literal",
            "start": 11,
            "end": 12,
            "value": 1,
            "raw": "1"
          }
        }
      ],
      "kind": "let"
    }
  ],
  "sourceType": "module"
}
```
## Babel 的处理步骤
Babel 的三个主要处理步骤分别是： 解析（parse），转换（transform），生成（generate）。

### 解析
解析步骤接收代码并输出 AST。 这个步骤分为两个阶段：**词法分析（Lexical Analysis） **和 语法分析（Syntactic Analysis）。

**词法分析** 

词法分析阶段把字符串形式的代码转换为 令牌（tokens） 流。

你可以把令牌看作是一个扁平的语法片段数组：
```
n * n;
```
```
[
  { type: { ... }, value: "n", start: 0, end: 1, loc: { ... } },
  { type: { ... }, value: "*", start: 2, end: 3, loc: { ... } },
  { type: { ... }, value: "n", start: 4, end: 5, loc: { ... } },
  ...
]
```
每一个 type 有一组属性来描述该令牌：

```
{
  type: {
    label: 'name',
    keyword: undefined,
    beforeExpr: false,
    startsExpr: true,
    rightAssociative: false,
    isLoop: false,
    isAssign: false,
    prefix: false,
    postfix: false,
    binop: null,
    updateContext: null
  },
  ...
}
```
和 AST 节点一样它们也有 start，end，loc 属性。

**语法分析**

语法分析阶段会把一个令牌流转换成 AST 的形式。 这个阶段会使用令牌中的信息把它们转换成一个 AST 的表述结构，这样更易于后续的操作。

### 转换
转换步骤接收 AST 并对其进行遍历，在此过程中对节点进行添加、更新及移除等操作。 这是 Babel 或是其他编译器中最复杂的过程 同时也是插件将要介入工作的部分。

### 生成
代码生成步骤把最终（经过一系列转换之后）的 AST 转换成字符串形式的代码，同时还会创建源码映射（source maps）。代码生成其实很简单：深度优先遍历整个 AST，然后构建可以表示转换后代码的字符串。

## 遍历
想要 **转换 AST** 你需要进行递归的树形遍历。

### Visitors（访问者）
当我们谈及“进入”一个节点，实际上是说我们在访问它们， 之所以使用这样的术语是因为有一个访问者模式（visitor）的概念。
```
const MyVisitor = {
  Identifier() {
    console.log("Called!");
  }
};

// 你也可以先创建一个访问者对象，并在稍后给它添加方法。
let visitor = {};
visitor.MemberExpression = function() {};
visitor.FunctionDeclaration = function() {}

function square(n) {
  return n * n;
}
```
这是一个简单的访问者，把它用于遍历中时，每当在树中遇见一个 Identifier 的时候会调用 Identifier() 方法。所以在下面的代码中 Identifier() 方法会被调用四次（包括 square 在内，总共有四个 Identifier）。
```
path.traverse(MyVisitor);
Called!
Called!
Called!
Called!
```
这些调用都发生在进入节点时，不过有时候我们也可以在退出时调用访问者方法。

假设我们有一个树状结构：
```
- FunctionDeclaration
  - Identifier (id)
  - Identifier (params[0])
  - BlockStatement (body)
    - ReturnStatement (body)
      - BinaryExpression (argument)
        - Identifier (left)
        - Identifier (right)
```
当我们向下遍历这颗树的每一个分支时我们最终会走到尽头，于是我们需要往上遍历回去从而获取到下一个节点。 向下遍历这棵树我们进入每个节点，向上遍历回去时我们退出每个节点。

* 进入 FunctionDeclaration
  * 进入 Identifier (id)
  * 走到尽头
  * 退出 Identifier (id)
  * 进入 Identifier (params[0])
  * 走到尽头
  * 退出 Identifier (params[0])
  * 进入 BlockStatement (body)
  * 进入 ReturnStatement (body)
    * 进入 BinaryExpression (argument)
    * 进入 Identifier (left)
    * 走到尽头
    * 退出 Identifier (left)
    * 进入 Identifier (right)
    * 走到尽头
    * 退出 Identifier (right)
    * 退出 BinaryExpression (argument)
  * 退出 ReturnStatement (body)
  * 退出 BlockStatement (body)
* 退出 FunctionDeclaration

所以当创建访问者时你实际上有两次机会来访问一个节点。
```
const MyVisitor = {
  Identifier: {
    enter() {
      console.log("Entered!");
    },
    exit() {
      console.log("Exited!");
    }
  }
};
```
[Babel 插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-asts)
[AST 抽象语法树](http://jartto.wang/2018/11/17/about-ast/)
