# 几种现代编程语言的设计分析

## Introduction



## 约定

### 待分析编程语言的选择

### 待分析编程语言概要

| Language | Programming Paradigm | Type System | Compilation Mode | Memory Model | Release Date | Application Scenarios |
| ---------- | -------- | ------------ | -------- | -------- | -------- | -------- |
| Python     | Multi-paradigm | Dynamic, Strong | JIT      | GC       | 1991 | Web, Enterprise, Embedded |
| Java       | Multi-paradigm | Static, Strong | AOT&JIT  | GC       | 1995 | Web, Mobile, Enterprise |
| C++        | Multi-paradigm | Static, Weak | AOT      | Manual | 1983 | Mobile, Enterprise, Embedded |
| JavaScript | Multi-paradigm | Dynamic, Weak | JIT      | GC       | 1995 | Web |
| Go         | Multi-paradigm | Static, Strong | AOT      | GC       | 2009 | Web, Enterprise |
| Swift      | Multi-paradigm | Static, Strong | AOT      | ARC | 2014 | Mobile, Enterprise |
| Dart       | Multi-paradigm | Static, Strong | AOT&JIT | GC | 2011 | Web, Mobile |
| Rust       | Multi-paradigm | Static, Strong | AOT      | Ownership | 2015 | Web, Enterprise, Embedded |
| Kotlin     | Multi-paradigm | Static, Strong | AOT&JIT | GC | 2016 | Web, Mobile |

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/type.png)

```latex
\figure{二维坐标-编程语言类型系统}
```

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/compilation.png)

```latex
\figure{图-编程语言编译模式}
```
![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/memory.png)

```latex
\figure{树-编程语言内存模型}	
```

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/timeline-16534866394691.png)

```latex
\figure{一维坐标-编程语言发布时间}
```

### 语言设计评价标准

+ 语法设计
	+ 高表达能力
	+ 高可靠性
	+ 高语义一致性

+ 编译方式设计
	+ 低内存开销
	+ 低时间开销

## 语法设计分析


### 编程范式支持程度


![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/Languages-paradigms-and-concepts.png)

```latex
\figure{Languages, paradigms, andconcepts}\cite{van2009programming}
```

#### 分析结果

该图大致描述了不同编程语言对函数式编程和面向对象编程概念的支持程度。对于特定的编程语言，在图中越靠近FP/OOP，它对FP/OOP的支持程度就越好。

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/paradigm.png)

```latex
\figure{二维坐标-编程语言编程范式}
```

#### 函数式编程概念表

| Concept                 | Meaning                                                      |
| ----------------------- | ------------------------------------------------------------ |
| Higher-order functions  | Take functions as variables, which can pass as parameters or return values. |
| Nested functions        | A function can define within another function.               |
| Anonymous functions     | A function can define without an identifier, including lambda expression, code block, etc. |
| Partial application     | Given a function with certain parameters, producing another function (with fewer parameters). |
| Closures                | A record stores a function together with an environment`\cite{sussman1998scheme}`. |
| Type inference          | The automatic detection of the type of an expression at compile time. |
| Pattern matching        | Dispatch branch by matching the pattern of a given sequence of tokens. |
| Statement as expression | Take all statements as expressions, which creates a definite value. |

#### 函数式编程概念支持表

| Language   | Higher-order functions | Nested functions | Anonymous functions | Partial application | Closures | Type inference | Pattern matching | Statements as expressions |
| ---------- | ---------------------- | ---------------- | ------------------- | ------------------- | -------- | -------------- | ---------------- | ------------------------- |
| Python     | √                      | √                | √                   | Python2.5           | √        | √              | Python3.10       | ×                         |
| Java       | Java8                  | ×                | Java8               | Java8               | Java8    | Java10         | ×                | ×                         |
| C++        | C++11                  | ×                | C++11               | C++11               | C++11    | C++11          | ×                | ×                         |
| JavaScript | √                      | √                | √                   | ECMAScript5         | √        | √              | ×                | ×                         |
| Go         | √                      | ×                | √                   | √                   | √        | √              | ×                | ×                         |
| Swift      | √                      | √                | √                   | √                   | √        | √              | √                | ×                         |
| Dart       | √                      | √                | √                   | √                   | √        | √              | ×                | ×                         |
| Rust       | √                      | √                | √                   | √                   | √        | √              | √                | √                         |
| Kotlin     | √                      | √                | √                   | √                   | √        | √              | √                | √                         |

#### 面向对象编程概念表

| Concept              | Meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| Encapsulation        | Hide the properties and implementation details of the object, and only expose the interface to the outside. |
| Inheritance          | Create classes that are built upon existing classes`\cite{johnson1988designing}`. A way of code reuse. |
| Composition          | Combine entities into more complex ones. A way of code reuse. |
| Delegation           | One entity passing something to another entity`\cite{wilkinson2009grid}`. A way of code reuse. |
| Polymorphism         | Use of a single symbol to represent multiple different types`\cite{cardelli1985understanding}`. |
| Everything is object | All the basic elements of a programming language are represented in the form of objects. |

#### 面向对象编程概念支持表

| Language   | Encapsulation | Inheritance | Composition | Delegation | Polymorphism | Everything is object |
| ---------- | ------------- | ----------- | ----------- | ---------- | ------------ | -------------------- |
| Python     | √             | √           | √           | √          | √            | √                    |
| Java       | √             | √           | √           | ×          | √            | ×                    |
| C++        | √             | √           | √           | ×          | √            | ×                    |
| JavaScript | √             | √           | √           | ×          | √            | √                    |
| Go         | √             | ×           | √           | √          | √            | ×                    |
| Swift      | √             | √           | √           | ×          | √            | √                    |
| Dart       | √             | √           | √           | ×          | √            | √                    |
| Rust       | √             | ×           | √           | √          | √            | √                    |
| Kotlin     | √             | √           | √           | √          | √            | √                    |


#### 其他语法概念表

| Concept | Meaning |
| ------- | ------- |
| Destructuring assignment | A way to unpack entity with hierarchical structure into distinct variables. |
| Null safety | A mechanism to prevent behavior that may cause a null pointer exception at compile time. |
| String interpolation | A way to insert expressions into string literals. |

#### 其他语法概念支持表


| Language   | Destructuring assignment | Null safety | String interpolation |
| ---------- | ------------------------ | ----------- | -------------------- |
| Python     | √                        | ×           | Python3.6            |
| Java       | ×                        | Java8       | ×                    |
| C++        | C++17                    | C++17       | ×                    |
| JavaScript | ECMAScript6              | ×           | ECMAScript6          |
| Go         | ×                        | ×           | ×                    |
| Swift      | ×                        | √           | √                    |
| Dart       | ×                        | √           | √                    |
| Rust       | √                        | √           | ×                    |
| Kotlin     | √                        | √           | √                    |


## 编程方式设计分析

### binary-trees

```latex
\begin{tabular}{lrrrr}
\toprule
{} &   n &  size(B) &   cpu(s) &  mem(KB) \\
lang      &     &          &          &          \\
\midrule
cpp-clang &  21 &      654 &   16.438 &   263580 \\
cpp-gcc   &  21 &      654 &   22.181 &   263620 \\
dart-aot  &  21 &     1212 &   45.461 &   799012 \\
dart-jit  &  21 &     1212 &   61.531 &  1626352 \\
go        &  21 &      482 &   50.955 &   220548 \\
java      &  21 &      552 &    5.607 &  2015512 \\
js-node   &  21 &      711 &   36.391 &  1130788 \\
python3   &  21 &      589 &  169.912 &   442180 \\
rust      &  21 &      751 &    7.796 &   132508 \\
swift     &  21 &      714 &   63.608 &   733144 \\
\bottomrule
\end{tabular}
```

```latex
\begin{tabular}{lrrrr}
\toprule
{} &   n &  size(B) &  cpu(s) &  mem(KB) \\
lang      &     &          &         &          \\
\midrule
cpp-clang &  14 &      654 &   0.087 &     1200 \\
cpp-gcc   &  14 &      654 &   0.104 &     3928 \\
dart-aot  &  14 &     1212 &   0.120 &     1680 \\
dart-jit  &  14 &     1212 &   0.644 &   170008 \\
go        &  14 &      482 &   0.214 &     7232 \\
java      &  14 &      552 &   0.155 &    47968 \\
js-node   &  14 &      711 &   0.717 &    90512 \\
python3   &  14 &      589 &   0.911 &    14420 \\
rust      &  14 &      751 &   0.042 &     1176 \\
swift     &  14 &      714 &   0.299 &    17616 \\
\bottomrule
\end{tabular}
```

### n-nody 

```latex
\begin{tabular}{lrrrr}
\toprule
{} &         n &  size(B) &   cpu(s) &  mem(KB) \\
lang      &           &          &          &          \\
\midrule
cpp-clang &  50000000 &     1173 &    5.926 &     1236 \\
cpp-gcc   &  50000000 &     1173 &    7.555 &     1228 \\
dart-aot  &  50000000 &     1266 &   10.220 &     9308 \\
dart-jit  &  50000000 &     1266 &   13.193 &   143436 \\
go        &  50000000 &     1310 &    6.581 &     1128 \\
java      &  50000000 &     1430 &    7.816 &    37260 \\
js-node   &  50000000 &     1268 &    8.550 &    39956 \\
python3   &  50000000 &     1196 &  541.319 &     7780 \\
rust      &  50000000 &     1480 &    5.818 &     1024 \\
swift     &  50000000 &     1192 &    9.585 &     6308 \\
\bottomrule
\end{tabular}
```

```latex
\begin{tabular}{lrrrr}
\toprule
{} &        n &  size(B) &  cpu(s) &  mem(KB) \\
lang      &          &          &         &          \\
\midrule
cpp-clang &  5000000 &     1173 &   0.621 &     1204 \\
cpp-gcc   &  5000000 &     1173 &   0.766 &     1204 \\
dart-aot  &  5000000 &     1266 &   1.033 &     9216 \\
dart-jit  &  5000000 &     1266 &   1.918 &   143256 \\
go        &  5000000 &     1310 &   0.661 &      816 \\
java      &  5000000 &     1430 &   0.880 &    37460 \\
js-node   &  5000000 &     1268 &   0.941 &    39864 \\
python3   &  5000000 &     1196 &  52.530 &     7800 \\
rust      &  5000000 &     1480 &   0.579 &     1020 \\
swift     &  5000000 &     1192 &   0.976 &     6308 \\
\bottomrule
\end{tabular}
```

### mandelbrot

```latex
\begin{tabular}{lrrrr}
\toprule
{} &      n &  size(B) &   cpu(s) &  mem(KB) \\
lang      &        &          &          &          \\
\midrule
cpp-clang &  16000 &      822 &   13.900 &    30172 \\
cpp-gcc   &  16000 &      822 &   13.931 &    28696 \\
dart-aot  &  16000 &      454 &  154.089 &    17240 \\
dart-jit  &  16000 &      454 &  151.501 &   148144 \\
go        &  16000 &      823 &   19.598 &    32412 \\
java      &  16000 &      665 &   27.834 &    34264 \\
js-node   &  16000 &      373 &  130.638 &    42020 \\
python3   &  16000 &      688 &  702.599 &    47780 \\
rust      &  16000 &      868 &   11.904 &    38528 \\
swift     &  16000 &      394 &   26.277 &     6200 \\
\bottomrule
\end{tabular}
```

```latex
\begin{tabular}{lrrrr}
\toprule
{} &     n &  size(B) &  cpu(s) &  mem(KB) \\
lang      &       &          &         &          \\
\midrule
cpp-clang &  4000 &      822 &   0.880 &     1552 \\
cpp-gcc   &  4000 &      822 &   0.881 &     1192 \\
dart-aot  &  4000 &      454 &  10.276 &    17380 \\
dart-jit  &  4000 &      454 &  10.096 &   148084 \\
go        &  4000 &      823 &   1.260 &     2420 \\
java      &  4000 &      665 &   1.827 &    34352 \\
js-node   &  4000 &      373 &   8.763 &    42540 \\
python3   &  4000 &      688 &  46.273 &    12172 \\
rust      &  4000 &      868 &   0.757 &     4388 \\
swift     &  4000 &      394 &   1.661 &     6240 \\
\bottomrule
\end{tabular}
```

