# 几种现代编程语言的设计分析

## 献诗

> Language design is cast in stone, 
> but this stone is reasonably soft, 
> and with some effort we can reshape it later.

## 现代编程语言

### 什么是现代编程语言

事实上，对于现代编程语言并没有一个统一的定义。不同的研究者通过不同的方面来定义它，例如找出软件工程和现代编程语言发展的联系`\cite{ModernProgrammingLanguagesSoftwareEnginerring}`，或是围绕着“抽象”来阐述`\cite{ModernProgrammingLanguagesAbstraction}`。上面的这些说法在各自适用范围内都是正确的，而这里将其适用的范围再度扩大，通过研究多种定义的交集，得出更加普适的现代编程语言定义。一个显而易见的事实是，随着编程语言实践的不断加深，现代编程语言的定义也在不断发生变化，因为编程语言是实践和理论紧密结合的理论。尽管其定义不断发生变化，但是有些核心内容却是始终不变的。这里给出现代编程语言的几个特性。

1. 实质大于形式。它应当提供描述性的语法结构，而不是手把手告诉机器应当做什么。实质是对机器功能的抽象。
2. 语义一致性。对于相似的语法结构，应当有相似的语法特性。
3. 语法自举。对于非核心的语法特性，在遵循语义一致性的基础上，应当由核心的语法特性组合而成。
4. 范式融合。提供多种编程范式，而不强迫用户使用特定的编程范式。
5. 语法检查。对于潜在的错误，应当在编译期就予以阻止，而不是在运行时出错再进行报告。

### 现代编程语言的更替

编程语言具有相当程度的时效性。对于时间跨度较大的不同时代的不同编程语言，虽然它们都符合上述的现代编程语言的特性，但是其中一方相较于另一方仍然是较为“过时”。这是由于上述原则在时间维度上是冲突的。事实上，实质大于形式要求编程语言抽象出符合具体应用场景的语法特性，而应用场景的需求会随着时间逐渐变化，因为应用场景经过技术迭代会进行技术升级。此时若想在原有的编程语言中加入新的语法特性，这很容易就破坏了语法自举和语义一致性的原则。因此，此时较好的解决方案是设计一门新的编程语言，从语言设计的层面上考虑满足新的需求。

但是，还有一些因素阻碍了编程语言淘汰的进度。编程语言从原型到大规模应用需要较长的时间，在这段时间里，编程语言会逐步完善自己的编程语言生态。编程语言生态就像一张密密麻麻的网，一旦编程语言开始大规模应用，想要将编程语言完全淘汰几乎是不可能的。对于发展中的计算机应用系统，最理想的状态是新的编程语言可以兼容旧的编程语言，从而使得新的编程语言可以继承旧的编程语言的生态，一个典型的例子是C和C++。但是，由于某些不可抗力的存在，很多时候新的编程语言无法兼容旧的编程语言，此时可以通过虚拟机实现编程语言生态的兼容，但是仅限于新的编程语言和旧的编程语言都是基于同一种虚拟机的语言，一个典型的例子是Java和Kotlin。此外，还有一种较为普适的方法是，新编程语言实现旧编程语言的数据接口，大部分语言和C/C++的交互都是这样完成的，但这样做会有较大的功能限制。最后，是一种不依赖于编程语言具体实现的方法，通过引入一个第三方协议来实现低耦合兼容，典型的例子是各个语言的RPC框架。各个编程语言都兼容了一些过去的编程语言，这也是必然的。

淘汰旧的编程语言仍是趋势，不过会以较慢的方式进行。通过对旧的编程语言兼容的方式虽然可以复用过去的过去的编程语言生态，但是由旧编程语言引起的维护性难题仍无法解决。因此，兼容旧的编程语言是一种妥协的做法，先通过旧的编程语言生态发展新的编程语言本身，再以新的编程语言重构旧的编程语言生态，最终实现编程语言的大规模淘汰。

## 编程语言分析简介

### 待分析编程语言的选择

对于编程语言的选择，首要关注的是当下最流行的趋势，因此选择的编程语言要能覆盖最常用的几种。其次，编程语言有许多划分标准，对于每一种划分标准，所选择的编程语言都应当覆盖大部分的选项。然后，应当着重突出近年来具有优秀设计的新生的现代编程语言，这些编程语言吸取了传统编程语言的优点，并修改了其中固有的缺点。从中可以看出这些年来应用导向的编程语言的发展趋势。

根据IEEE spectrum`\cite{IEEETopProgrammingLanguages}`的数据，2021年最流行的前五个最流行的编程语言正是Python、Java、C、C++和JavaScript。但是对于C，它是一种偏向于计算机底层的语言，并不符合现代编程语言的原则。事实上，严格来说，C语言的语法设计不符合现代编程语言原则的任何一条。因此，将C语言排除，剩余的语言加入研究的范畴。而对于剩余的四种编程语言，以当今的眼光看待，现代编程语言的原则并不是完全符合。但是在这些语言刚刚诞生的时代，这些语言都是作为现代编程语言的代表而出现的。因此将这四种传统的编程语言与新兴的现代编程语言进行对比。

根据上述的编程现代编程语言标准选择出数种编程语言，并按照流行顺序进行排列`\cite{IEEETopProgrammingLanguages}`，得到的是Go、Swift、Dart、Rust和Kotlin（其中Swift和Dart流行程度相当）。巧合的是，这些语言的也是按照这个顺序先后发布。另外，这些语言的类型系统都是静态强类型，并且都支持AOT的编译方式，尽管不都是采用垃圾回收的内存管理方式，但是也都脱离了手动管理内存的方式。这些都是在现在的应用环境下，现代编程语言的普遍特征。

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

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/timeline.png)

```latex
\figure{一维坐标-编程语言发布时间}
```

### 语言设计评价标准

这里的评价标准是更加应用导向的，用以评价一个编程语言是否“实用”，而不是像“现代编程语言的原则”一样，评价一个编程语言是否“优雅”。事实上，大多数流行的语言并不优雅，这是因为编程语言的发展不仅仅是技术上的突破，而是涉及到政治、商业以及其他外部因素。一个典型的例子是JavaScript的发展历程。时至今日，主流浏览器的JavaScript的语言标准仍然不统一。但是这并不妨碍JavaScript是Web应用中最实用的语言。而评价语言的是否实用，则应当主要考察两个方面。一个方面是其语法设计。对于复杂的业务逻辑，需要编程语言提供很强的表达能力，将无关于业务逻辑的底层实现隔离，以适应快速变化的需求，还需要编程语言提供检查程序正确性和可靠性的方案。另一个方面是性能，编程语言在运行时总是被期望具有较低的内存开销和时间开销，其主要依靠编译时（本地语言）和运行时（虚拟机语言）的优化。

| 评价项目 | 具体含义                                                     | 相关内容                                   |
| -------- | ------------------------------------------------------------ | ------------------------------------------ |
| 表达能力 | 对于抽象且复杂的业务逻辑，编程语言能提供简洁的描述方式。     | 1.编程范式的支持程度 2.编码的字符数量      |
| 可维护性 | 按照标准编码规范完成业务逻辑后，后续还便于增加新功能或修复漏洞。 | 1.编程范式的支持程度                       |
| 可靠性   | 按照标准编码规范完成业务逻辑后，在极端条件下的不崩溃。       | 1.类型系统 2.编程范式的支持程度 3.语法检查 |
| 性能     | 将由该编程语言编写的系统部署在在目标机器上运行时，占用较少的硬件资源。 | 1.运行时间开销 2.运行内存开销              |

但实际上，编程语言设计无法准确的进行定量分析。原因有两个方面。第一个是，某些评价的标准不是定量的分析。对于表达能力和可靠性，两者都是对抽象概念的描述，反映在实践中有不可数种类的情况，无法全部考虑到。虽然可以设计定量的分析，但是必须通过限制某些特定的应用场景才能完成。第二个是，评价的标准不够充分。为了论文结构上的完整，放弃了一些难以测量的评价标准，如实时系统中的响应时间等。对于带有垃圾回收机制的编程语言，垃圾回收带来的性能损耗不可忽略不计。尤其是基于JVM的编程语言，在进行垃圾回收时会有“Stop the World”的问题，这些都会影响编程语言的性能。但是这里为了简便起见不予考虑。

## 编程范式

编程范式来源于人对现实世界认知方式，而编程语言是形式化描述这种认知方式的工具。人们在开发软件时，会将现实世界在计算机中进行模拟，不同的编程范式在于对现实世界建模方式的不同。对世界的建模往往需要解决两个问题。第一是找出世界的基本单位是什么，称之为一等公民。这些一等公民可以存储在变量中，可以作为函数参数和函数返回值被传递，简而言之，就是程序中要被操作的“数据”。第二是找出这些基本单位之间的关系，称之为计算。计算在程序中就可以看作对数据的“操作”。例如面向对象将对象看作一等公民，对象之间的计算称之为方法。再例如函数式将函数看作一等公民，而计算的就是lambda演算，是对函数本身的计算。有趣的是，函数式中函数既是基本单位，又是计算的形式。

编程范式是编程语言最底层的设计。从理论上来说，编程语言需要帮助人们完成对现实世界的建模，因此需要考虑应当提供哪些建模的基础设施，即是编程语言的编程范式。从实践上来说，设计编程语言应当首先考虑一等公民，作为编程语言的基本单位。不同的编程范式之间存在一定的冲突，来源于一等公民的冲突。尤其是对确定范式的编程语言增加新的范式，是非常困难的。一个典型的例子是Java。Java8增加的函数式特性，并不能从根本上改变Java的编程范式，顶多算是语法糖。由于缺少最初的编程范式设计，Java难以完全兼容函数式特性，因此Java中的函数式是残缺的，不优雅的。因此，对于编程语言，尤其是对于多范式的现代编程语言，首先应当确定编程范式，进而确定一等公民。


### 编程语言、范式和概念


![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/Languages-paradigms-and-concepts.png)

```latex
\figure{Languages, paradigms, and concepts}\cite{van2009programming}
```

 图`ref{fig:concept}`展示了语言，范式和概念之间的关系。每个范式包含一组核心的概念，同时，每个语言也实现了多种范式`\cite{van2009programming}`。

### 函数式与面向对象

函数式和面向对象是现代编程语言中通常具备的编程范式。而各个编程语言对它们的支持程度却并不相同，从表中可以看出各个语言对函数式和面向对象概念的支持程度。

对于组成编程范式的概念，这里选择更加偏向于实践的组织方式。它将更偏向于选择具体编程语言中定义的概念，而不是编程范式理论中定义的概念。例如，对于函数式概念中的嵌套函数和匿名函数，它们在很多编程语言中被实现为lambda表达式。再比如，对于面向对象概念中的组合，目前流行的大多数编程语言都支持它。事实上，对于编程语言设计本身来说，组合仅仅是安排数据的一种方式，它并不属于任何一种范式。甚至像较为古老的语言——C语言，都提供了结构组合和函数组合。因此，并不将其纳入分析。

为了更加准确地区分各个语言编程范式的强弱，引入了一些并不常用的概念。这些概念都是属于特定的编程范式，但是通常在讨论多范式编程语言的时候，并不把这种概念当作核心概念，因为这些概念在范式融合的过程中较难实现。例如“statements as expressions”，这一概念在函数式语言中较为常见，它实现了表达式和语句语义上的统一，但大多数面向对象和函数式范式融合的编程语言中都没有实现这一概念。随着编程语言的不断发展，Kotlin和Rust实现了包含此概念的函数式与面向对象范式的融合，因此可以将Kotlin和Rust视为具有更高层次的函数式范式。另一个例子是“everything is object”。根据面向对象的定义，它本应当是面向对象中最核心的概念。但事实上，早期的编程语言往往带有大量的指令式特征，并不是像编程语言中的所有元素都看作对象。例如Java中仍然存在“primitive data types”，它们不是对象，因此我们不能像使用对象一样调用这些类型的方法。或许“primitive data types”有许多性能上的正面因素，但是从语义一致性的角度来讲，“primitive data types”是负面效果的。因此具有“everything is object”概念的语言被认为具有更好的面向对象特征。

而对于面向对象的核心概念，流行语言的支持程度并没有太多的差距。由于其发展历史较为久远，并且近些年来理论上和实践上并没有大的创新，因此大多数的编程语言已经相对完整地实现了面向对象的核心概念。另外，越来越多的编程语言通过支持组合和委托来实现面向对象特性，而单纯的继承已经被证明是糟糕的实践。因此，Go和Rust中抛弃了类和继承的概念，通过trait来实现传统的面向对象。相较于基于class的面向对象，基于trait的面向对象具有更松散的耦合和更灵活的实现。不过，大多数语言都同时支持trait和class，以实现不同粒度的控制。

编程语言的范式与其应用场景有非常密切的关系。尤其是对于单一应用领域的编程语言。一个典型的例子是Dart。它的主要应用场景是Web，通常作为图形用户界面框架Flutter的支持语言。显然易见的是，我们使用的大多数图形用界面框架都有复杂的继承结构。这是因为，对于图形用户界面，大多数应用场景都满足里氏代换原则，即子类型能够完全替换掉夫类型。这是使用继承的充分条件。因此，Dart仅仅提供了基于继承的面向对象概念。另一个例子是Java。Java早年间用于Enterprise开发，为了提高代码的复用性，提供了面向对象范式。后来用于Web服务器，对业务逻辑的抽象有着很高的要求，因此额外增加了函数式的概念。

随着编程语言实践的不断加深，编程语言对函数式概念的支持程度越来越高。有了对于上世纪八九十年代发布的编程语言，编程语言刚诞生时，对函数式的支持并不好。尤其是Java和C++，从最初的语法来看，它们在设计时没有考虑到要实现函数式编程。而对于JavaScript和Python来说则好一些，它们天生支持函数式，但仍缺少了一些高级的函数式概念。对于2010年前后发布的编程语言，则都支持了函数式的基本概念。特别是Rust和Kotlin，还额外支持了“Statements as expressions”的概念，这是现代编程语言范式融合发展上的里程碑。

综上所述，该图大致描述了不同编程语言对函数式编程和面向对象编程概念的支持程度。对于特定的编程语言，在图中越靠近FP/OOP，它对FP/OOP的支持程度就越好。

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/paradigm.png)

```latex
\figure{二维坐标-编程语言编程范式}
```



| Concept                 | Meaning                                                      |
| ----------------------- | ------------------------------------------------------------ |
| Higher-order functions  | Take functions as variables, which can pass as parameters or return values. |
| Lambda expression       | A short block of code which takes in parameters and returns a value. |
| Partial application     | Given a function with certain parameters, producing another function (with fewer parameters). |
| Closures                | A record stores a function together with an environment`\cite{sussman1998scheme}`. |
| Type inference          | The automatic detection of the type of an expression at compile time. |
| Pattern matching        | Dispatch branch by matching the pattern of a given sequence of tokens. |
| Statement as expression | Take all statements as expressions, which creates a definite value. |

`\table{函数式编程概念表}`

| Language   | Higher-order functions | Lambda expression | Partial application | Closures | Type inference | Pattern matching | Statements as expressions |
| ---------- | ---------------------- | ----------------- | ------------------- | -------- | -------------- | ---------------- | ------------------------- |
| Python     | √                      | √                 | Python2.5           | √        | √              | Python3.10       | ×                         |
| Java       | Java8                  | Java8             | Java8               | Java8    | Java10         | ×                | ×                         |
| C++        | C++11                  | C++11             | C++11               | C++11    | C++11          | C++17            | ×                         |
| JavaScript | √                      | √                 | ECMAScript5         | √        | √              | ECMAScript6      | ×                         |
| Go         | √                      | √                 | √                   | √        | √              | ×                | ×                         |
| Swift      | √                      | √                 | √                   | √        | √              | √                | ×                         |
| Dart       | √                      | √                 | √                   | √        | √              | ×                | ×                         |
| Rust       | √                      | √                 | √                   | √        | √              | √                | √                         |
| Kotlin     | √                      | √                 | √                   | √        | √              | √                | √                         |

`\table{函数式编程概念表}`

| Concept              | Meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| Encapsulation        | Hide the properties and implementation details of the object, and only expose the interface to the outside. |
| Inheritance          | Create classes that are built upon existing classes`\cite{johnson1988designing}`. A way of code reuse. |
| Composition          | Combine entities into more complex ones. A way of code reuse. |
| Delegation           | One entity passing something to another entity`\cite{wilkinson2009grid}`. A way of code reuse. |
| Traits               | A set of method conventions. Broadly including trait, interface, protocol and mixin. |
| Polymorphism         | Use of a single symbol to represent multiple different types`\cite{cardelli1985understanding}`. |
| Everything is object | All the basic elements of a programming language are represented in the form of objects. |

`\table{面向对象编程概念表}`

| Language   | Inheritance | Delegation | Traits | Polymorphism | Everything is object |
| ---------- | ----------- | ---------- | ------ | ------------ | -------------------- |
| Python     | √           | √          | ×      | √            | √                    |
| Java       | √           | ×          | √      | √            | ×                    |
| C++        | √           | ×          | √      | √            | ×                    |
| JavaScript | √           | √          | ×      | √            | √                    |
| Go         | ×           | ×          | √      | √            | ×                    |
| Swift      | √           | ×          | √      | √            | √                    |
| Dart       | √           | ×          | ×      | √            | √                    |
| Rust       | ×           | √          | √      | √            | √                    |
| Kotlin     | √           | √          | √      | √            | √                    |

`\table{面向对象编程概念表}`



## 类型系统

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/type.png)

```latex
\figure{二维坐标-编程语言类型系统}
```


## 编程语言性能

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/compilation.png)

```latex
\figure{图-编程语言编译模式}
```
![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/memory.png)

```latex
\figure{树-编程语言内存模型}	
```

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

