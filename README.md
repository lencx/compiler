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
* `上下文无关文法(CFG)«Context-free Grammar»`
* `非终结符«Non-terminal»`
* `产生式«Production Rule»`
* `终结符«Terminal»`: 只有终结符才可以成为 AST 的叶子节点
* `推导«Derivation»`
* `枚举«enum»`
* `递归算法`: 是很好的自顶向下解决问题的方法，是计算机领域的一个核心的思维方式
* `二元表达式«Binary Expression»`
* `左递归«Left Recursive»`
* `优先级«Priority»`
* `结合性«Associativity»`
* `巴科斯范式«BNF»`
* `扩展巴科斯范式«EBNF»`: 它跟普通的 BNF 表达式最大的区别，就是里面会用到类似正则表达式的一些写法
* `长方形«rectangle»`
* `中心点«center»`
* `尾递归`: 尾递归函数的最后一句是递归地调用自身。编译程序通常都会把尾递归转化为一个循环语句。相对于递归调用来说，循环语句对系统资源的开销更低，因此，把尾递归转化为循环语句也是一种编译优化技术
* ...

## Summary

* 词法分析是把程序分割成一个个 Token 的过程，可以通过构造有限自动机来实现。
* 语法分析是把程序的结构识别出来，并形成一棵便于由计算机处理的抽象语法树。可以用递归下降的算法来实现。
* 语义分析是消除语义模糊，生成一些属性信息，让计算机能够依据这些信息生成目标代码。
* 要实现一个词法分析器，首先需要写出每个词法的正则表达式，并画出有限自动机，之后，只要用代码表示这种状态迁移过程就可以了。
* 了解上下文无关文法，知道它能表达主流的计算机语言，以及与正则文法的区别。
* 理解递归下降算法中的“下降”和“递归”两个特点。它跟文法规则基本上是同构的，通过文法一定能写出算法。
* 通过遍历 AST 对表达式求值，加深对计算机程序执行机制的理解。
* 优先级是通过在语法推导中的层次来决定的，优先级越低的，越先尝试推导。
* 结合性是跟左递归还是右递归有关的，左递归导致左结合，右递归导致右结合。
* 左递归可以通过改写语法规则来避免，而改写后的语法又可以表达成简洁的 EBNF 格式，从而启发我们用循环代替右递归。

---

```bash
# 编译器的整个编译过程
    ┌----------- [前端] -----------┐
[词法分析] ---> [语法分析] ---> [语义分析]
                                  ↓
                            [生成中间代码] ------> [优化] ---> [生成目标代码]
                                └---[有时叫“中端”]---┘               |
                                └-------------- [后端] -------------┘
```

```bash
# 算术表达式的文法规则
add -> mul | add + mul
mul -> pri | mul * pri
pri -> Id | Num | (add)

#  2+3*5                    2+3+5
    add                      add
    ╱|╲                      ╱|╲
add  +  mul              add  +   mul
 |      ╱|╲              ╱|╲       |
mul  mul * pri       add  +  mul  pri
 |    |     |         |       |    |
pri  pri   Num       mul     pri  Num
 |    |     5         |       |    5
Num  Num             pri     Num
 2    3               |       3
                     Num
                      2

# 1. 简化: add-add-mul-pri-Num ---> add-Num
# 2. Num、+ 和 * 都是终结符
# 3. 终结符都是词法分析中产生的 Token
# 4. 而那些非叶子节点，就是非终结符
# 5. 文法的推导过程，就是把非终结符不断替换的过程，让最后的结果没有非终结符，只有终结符

# 实际应用中常写成如下形式
# 巴科斯范式(BNF)
add ::= mul | add + mul
mul ::= pri | mul * pri
pri ::= Id | Num | (add)

# 扩展巴科斯范式(EBNF)
# 规则中运用了 * 号，来表示这个部分可以重复 0 到多次
# 这种写法跟标准的 BNF 写法是等价的，但是更简洁
# 为什么是等价的? 因为一个项多次重复，就等价于通过递归来推导
# 上下文无关文法包含了正则文法，比正则文法能做更多的事情
add -> mul (+ mul)*
```

```bash
# 确保正确的优先级
# 由加法规则推导到乘法规则，这种方式保证了 AST 中的乘法节点一定会在加法节点的下层，也就保证了乘法计算优先于加法计算
# 关系运算（>、=、<）放在加法的上层
# 逻辑运算（and、or）放在关系运算的上层
# 这里表达的优先级从低到高是：赋值运算、逻辑运算（or）、逻辑运算（and）、相等比较（equal）、大小比较（rel）、加法运算（add）、乘法运算（mul）和基础表达式（pri）
exp -> or | or = exp
or -> and | or || and
and -> equal | and && equal
equal -> rel | equal == rel | equal != rel
rel -> add | rel > add | rel < add | rel >= add | rel <= add
add -> mul | add + mul | add - mul
mul -> pri | mul * pri | mul / pri

# 我们在最低层，也就是优先级最高的基础表达式（pri）这里，用括号把表达式包裹起来，递归地引用表达式就可以了。
# 这样的话，只要在解析表达式的时候遇到括号，那么就知道这个是最优先的。这样的话就实现了优先级的改变
pri -> Id | Literal | (exp)

# 对于左结合的运算符，递归项要放在左边；而右结合的运算符，递归项放在右边
# 加法表达式的规则
add -> mul | add + mul

# 消除左递归
add -> mul add’
# 文法中，ε（读作 epsilon）是空集的意思
add’ -> + mul add’ | ε

# 2+3+5
# add’的规则是右递归
        add       --->     add   ↘︎          --->          ↗︎   add’
        ╱|╲                 ╱╲      ↘︎                   ↗︎      ╱╲
    add  +   mul        mul     add’   ↘︎              ↗︎    add’   mul
    ╱|╲       |          |      ╱|╲      ↘︎          ↗︎      ╱╲      |
add  +  mul  pri        pri   + mul  add’   ↘︎     ↗︎     add   mul pri
 |       |    |          |       |    ╱|╲                |     |   |
mul     pri  Num        Num     pri + mul ε             mul   pri Num
 |       |    5          2       |     |                 |     |   5
pri     Num                     Num   pri               pri   Num
 |       3                       3     |                 |     3
Num                                   Num               Num
 2                                     5                 2

# EBNF方式表达: 允许用 * 号和 + 号表示重复
add -> mul (+ mul)*
# 伪代码
# (+ mul)*可以写成循环
mul();
while(next token is +){
  mul()
  createAddNode
}
```
