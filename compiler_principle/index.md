# CompilerPrinciples

# 编译原理

## chapter 3 词法分析

- 目标：学习如何构建一个词法分析器
- 方法：
	- 手动：建立起每个词法单元的词法结构图或其他描述，编写代码识别输入中的每个词素，返回识别到的词法单元的有关信息
	- 自动：向一个词法分析器生成工具(lexical-analyzer generater)描述出词素的模式，然后将这些模式编译为具有词法分析器功能的代码
- 理论运用：
	- 正则表达式(regular expression)
	- 不确定有穷自动机(NFA)
	- 确定有穷自动机(DFA)
- 工具
    - Lex(lexicl-analyzer generator)

### 3.1 词法分析器的作用

- 主要任务
	读入源程序的输入字符、将它们组成词素，生成并输出一个词法单元序列，每个词法单元对应于一个词素

- 其他任务
	- 过滤掉源程序中的注释和空白
	- 将编译器生成的错误消息与源程序的位置联系起来

#### 3.1.1 词法分析和语法分析
把编译过程的部分划分为词法分析和语法分析的原因：
- 简化编译器的设计
- 提高编译器的效率
- 增强编译器的可移植性(抽象、分离)

#### 3.1.2 词法单元、模式和词素
- **词法**单元由一个词法单元名和一个可选的属性值组成(token, attribute)
- **模式**描述了一个词法单元的词素可能具有的形式
- **词素**是源程序的一个字符序列、和某个词法单元的模式匹配，并被词法分析器识别为该词法单元的一个实例

**理解**:可以把词法单元看成一个集合，模式看成构成这个集合的约束条件，词素看成这个集合里面的元素

#### 3.1.3 词法单元的属性值
如果多个词素可以和一个模式匹配，那么词法分析器必须向编译器和后续阶段提供有关被匹配词素的**附加信息**(词素)
假设词法单元至多有一个相关的属性值，这个属性值可能是一个组合了多种信息的**结构化数据**
一个**标识符**的属性值是一个指向符号表中该标识对应条目的**指针**

### 3.2 输入缓冲

#### 3.2.1 缓冲区对

#### 3.2.1 哨兵标记

### 3.3 词法单元的规约
**正则表达式**是一种描述词素模式的重要表示方法，利用正则表达式的**形式化表示**来描述**模式**

#### 3.3.1 串和语言
- **字母表(alphabet)**是一个有限的符号集合
- (某个字母表上的)**串(string)**是该字母表中符号的一个有穷序列
==注意==：**空串**长度为0的串用E表示
- **语言(language)**是某个给定字母表上一个任意的可数上串集合

==串的各部分术语==
- 串s的**前缀(prefix)**
- 串s的**后缀(suffix)**
- 串s的**子串(substring)**
- 串s的**真(true)前缀、真后缀、真子串**
- 串s的**子序列(subsequence)**
- 串x和y的**连接(concatenation)**(记作xy) 是把y附加到x后面而形成的串

#### 3.3.2 语言上的运算
在词法分析中，最重要的语言上的运算是 **并**、 **连接** 和 **闭包运算**

**上述运算的正式定义:**

*image*



#### 3.3.3 正则表达式
人们常常使用一种称为正则表达式的表示方法来描述语言：

**正则表达式可以描述所有通过对某个字母表上的符号应用这些运算符而得到的语言**
- - - -
正则表达式可以由较小的正则表达式按照如下规则递归地构建：

每个正则表达式 r 表示一个语言 L(r)，这个语言也是根据r的子表达式所表示的语言递归定义的

下面的规则定义了某个字母表E(求和符号)上的正则表达式以及这些表达式所表示的语言

#### 3.3.4 正则定义
为方便表示，可以 **给正则表达式命名**，并在之后的正则表达式中像使用符号一样使用这些名字
- - - -
如果(求和符号)是基本符号的集合，那么一个正则定义(regular definition)是具有如下形式的定义序列:

                d1 -->  r1
                d2 -->  r2
                   ...
                dn -->  rn
其中:
    - 每一个di都是新符号，他们都不在基本符号集中，并且各不相同
    - 每一个ri都是字母表E U {d1, d2, ... , di-1} 上的正则表达式
- - - -

#### 3.3.5 正则表达式的扩展(未看）


### 3.6 有穷自动机
Lex将一个其输入程序转换成一个词法分析器的核心是是被称为 **有穷自动机(finite automata)** 的表示方法

自动机在本质上是与状态转化图类似的图,但有几点不同：
- 有穷自动机是 **识别器(recogonizer)**,只能对每个可能的输入串简单地回答“是”或“否”
- 有穷自动机分为两类：
    - **不确定的有穷自动机(Nondeterministic Finite Automata,NFA)**  对其边上的标号没有任何限制。
    - 对于每个状态及自动机输入字母表中的每个符号， **确定的有穷自动机(Determinitic Finite Automata,DFA)** 有且只有一条离开该状态、以该符号为标号的边

确定的和不确定的有穷自动机能识别的语言的集合是相同的。事实上，这些语言的集合正好是能够用正则表达式描述的语言的集合。这个集合中的语言称为 **正则语言(regular language)**。

#### 3.6.1 不确定的有穷自动机

#### 3.6.2 转化表

#### 3.6.3 自动机中输入字符串的接受

#### 3.6.4 确定的有穷自动机

### 3.7 从正则表达式到自动机

#### 3.7.1 从NFA 到DFA 的转换
子集构造法

#### 3.7.2 NFA 模拟

#### 3.7.3 NFA模拟的效率(未看)

#### 3.7.4 从正则表达式构造NFA

#### 3.7.4 字符串处理算法的效率(未看)


## chapter 4 语法分析

### 4.1 引论

#### 4.1.1 语法分析器的作用
- 语法分析器从词法分析器获得一个由词法单元组成的串，并验证这个串可以由源语言的文法生成；构造出一棵语法分析树，并将它传递给编译器其他部分进一步处理

- 处理文法的语法分析器大体上可以分为三种类型：
    - 通用的
    - 自顶向下的
    - 自底向上

- 最高效的自顶向下和自底向上方法只能处理某些文法子类，但其中的某些子类，特别是LL和LR文法，其表达能力足以描述现代程序设计语言的大部分语法构造

