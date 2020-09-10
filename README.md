# compiler

* `前端«Front End»`: 编译器对程序代码的分析和理解过程。它通常只跟语言的语法有关，跟目标机器无关。
* `后端«Back End»`: 生成目标代码的过程，跟目标机器有关。
* `词法分析«Lexical Analysis»`: 词法分析是把程序分割成一个个 Token 的过程，可以通过构造有限自动机来实现。
* `语法分析«Syntactic Analysis, or Parsing»`: 语法分析是把程序的结构识别出来，并形成一棵便于由计算机处理的抽象语法树。可以用递归下降的算法来实现。
* `语义分析«Semantic Analysis»`: 语义分析是消除语义模糊，生成一些属性信息，让计算机能够依据这些信息生成目标代码。
* `抽象语法树(AST)«Abstract Syntax Tree»`: 程序也有定义良好的语法结构，它的语法分析过程，就是构造这么一棵树。一个程序就是一棵树，这棵树叫做抽象语法树。树的每个节点（子树）是一个语法单元，这个单元的构成规则就叫“语法”。每个节点还可以有下级节点。
* `递归下降算法«Recursive Descent Parsing»`: 是一种自顶向下的算法。
* `词法记号«Token»`
* `等于«Equal»`
* `大于(GT)«Greater Than»`
* `大于等于(GE)«Greater Equal»`
* `小于(LT)«Less Than»`
* `小于等于(LE)«Less Equal»`
* `正则文法«Regular Grammar»`
* `正则表达式«Regular Expression»`
* `有限自动机«Finite-state Automaton，FSA，or Finite Automaton»`
* `枚举«enum»`
* ...

```bash
# 编译器的整个编译过程
    ┌----------- [前端] -----------┐
[词法分析] ---> [语法分析] ---> [语义分析]
                                  ↓
                            [生成中间代码] ------> [优化] ---> [生成目标代码]
                                └---[有时叫“中端”]---┘               |
                                └-------------- [后端] -------------┘
```
