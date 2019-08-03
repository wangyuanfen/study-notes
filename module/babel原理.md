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

[Babel 插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-asts)
