# 几种现代编程语言设计的分析

## 献诗

> Language design is cast in stone, 
>
> but this stone is reasonably soft, 
>
> and with some effort we can reshape it later.

## Abstract

以现代编程语言为基点，通过编程语言理论的几个方面——编程范式、类型系统、性能，对常见的编程语言进行了语言设计上的分析。

编程范式部分给出了评价编程范式支持程度的方法，并对现代编程语言的编程范式支持程度进行了评价，从而得到编程范式与应用场景的联系。

类型系统部分分析了现代编程语言的类型系统。

性能部分通过基准测试的方法，定量分析了现代编程语言的性能，并定性分析了编程语言的性能与应用场景的联系。

最后得到结论，编程语言的设计与实践是动态发展的，编程语言的设计与其应用场景有密切的联系。


## Introduction

在编程语言理论中，编程语言的设计一直是个重要课题。因为编程语言的设计综合了编程语言理论的各个分支，是编程语言最终实现的前置环节。而通过对现代编程语言的设计进行分析，可以得到编程语言发展的趋势，进而从应用的角度对编程语言设计进行反哺。

大体上看，编程语言设计的分析的文献的总体数量是比较少的。M Coblenz认为，编程语言设计应该从编程语言相关的多个理论进行考虑，并给出了一些编程语言设计的评价方式`\cite{coblenz2018interdisciplinary}`。A Stefika给出了编程语言设计的要考虑的一些问题，并认为类型系统对于编程语言是至关重要的`\cite{stefik2014programming}`。但上述成果注重于对编程语言设计的定性分析，缺少一些具体的分析案例。LA Meyerovich分析流行编程语言的使用情况，通过统计学给出编程语言设计的最佳实践`\cite{meyerovich2013empirical}`。这种分析方式在理论性和系统性方面有一定的不足。F Morandat对R语言进行了系统的分析，包括性能、语法设计和应用场景等`\cite{morandat2012evaluating}`。该文章采用了比较好的研究方法，可以在该文章的基础上，拓宽分析角度。当前，人们对于编程语言设计的分析缺少一个成体系的、兼具理论性和实践性、涉及范围广的、应用导向的分析。

本文通过分析现代编程语言的设计，得出不同编程语言因素对编程语言应用带来的影响。例如，随着编程语言的发展，现代编程语言的范式支持程度为何不断发生变化、现代编程语言中有怎样的类型系统、现代编程语言如何保证程序的高效，等等。

## 几种现代编程语言及其设计

事实上，对于现代编程语言并没有一个统一的定义。不同的研究者通过不同的方面来定义它，例如找出软件工程和现代编程语言发展的联系`\cite{ModernProgrammingLanguagesSoftwareEnginerring}`，或是围绕着“抽象”来阐述`\cite{ModernProgrammingLanguagesAbstraction}`。上面的这些定义在各自领域内都是正确的，而这里将其定义进行抽象和拓展，以适应更多领域。通过研究多种定义的交集，得出更加普适的现代编程语言定义。一个显而易见的事实是，随着编程语言实践的不断加深，现代编程语言的定义也在不断发生变化。这是因为编程语言是实践和理论紧密结合的理论。尽管其定义不断发生变化，但是有些核心内容却是始终不变的。这里给出现代编程语言的几个特性。

1. 实质大于形式。它应当提供描述性的语法结构，而不是手把手告诉机器应当做什么。实质是对机器功能的抽象。
2. 语义一致性。对于相似的语法结构，应当有相似的语法特性。
3. 语法自举。对于非核心的语法特性，在遵循语义一致性的基础上，应当由核心的语法特性组合而成。
4. 范式融合。提供多种编程范式，而不强迫用户使用特定的编程范式。

### 待分析编程语言的选择


对于编程语言的选择，首要关注的是当下最流行的趋势，因此选择的编程语言要能覆盖最常用的几种。其次，编程语言有许多分类标准，对于每一种划分标准，所选择的编程语言都应当覆盖大部分的选项。然后，应当着重突出近年来具有优秀设计的新生的现代编程语言，这些编程语言吸取了过去编程语言的优点，并改进了其中固有的缺点。从这些被挑选的编程语言中，可以看出来这些年来应用导向的编程语言的发展趋势。

根据IEEE spectrum`\cite{IEEETopProgrammingLanguages}`的数据，2021年最流行的前五个最流行的编程语言正是Python、Java、C、C++和JavaScript。一个特例是C，它被广泛用于编译以提供对计算机系统的低级访问。准确来说，C语言的语法设计不符合现代编程语言原则的任何一条。而对于上述剩余的四种流行编程语言，它们尽管并不是完全符合现代编程语言的原则，但是仍有帮助。在这些语言刚刚诞生的时代，这些语言都是作为现代编程语言的代表而出现的。因此将这四种传统的编程语言与新兴的现代编程语言进行对比。

![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/timeline.png)

根据上述的现代编程语言特性选择出数种编程语言，并按照流行顺序进行排列`\cite{IEEETopProgrammingLanguages}`，得到的是Go、Swift、Dart、Rust和Kotlin（其中Swift和Dart流行程度相当）。巧合的是，这些语言的也是按照这个顺序先后发布。另外，这些语言的类型系统都是相似的，并且都支持AOT的编译方式，尽管不都是采用垃圾回收的内存管理方式，但是也都脱离了手动管理内存的方式。这些都是在现在的应用环境下，现代编程语言的普遍特征。

| Language   | Programming Paradigm | Type System           | Compilation Mode | Memory Model | Release Date | Application Scenarios        |
| ---------- | -------------------- | --------------------- | ---------------- | ------------ | ------------ | ---------------------------- |
| Python     | Multi-paradigm       | Dynamically, Strongly | JIT              | GC           | 1991         | Web, Enterprise, Embedded    |
| Java       | Multi-paradigm       | Statically, Strongly  | AOT              | GC           | 1995         | Web, Mobile, Enterprise      |
| C++        | Multi-paradigm       | Statically, Weakly    | AOT              | Manual       | 1983         | Mobile, Enterprise, Embedded |
| JavaScript | Multi-paradigm       | Untyped               | JIT              | GC           | 1995         | Web                          |
| Go         | Multi-paradigm       | Statically, Strongly  | AOT              | GC           | 2009         | Web, Enterprise              |
| Swift      | Multi-paradigm       | Statically, Strongly  | AOT              | ARC          | 2014         | Mobile, Enterprise           |
| Dart       | Multi-paradigm       | Statically, Strongly  | AOT&JIT          | GC           | 2011         | Web, Mobile                  |
| Rust       | Multi-paradigm       | Statically, Strongly  | AOT              | Ownership    | 2015         | Web, Enterprise, Embedded    |
| Kotlin     | Multi-paradigm       | Statically, Strongly  | AOT&JIT          | GC           | 2016         | Web, Mobile                  |

### 如何评价编程语言的设计

有两个主要方面可以决定语言是否有用。一个方面是其语法设计。对于复杂的业务逻辑，需要编程语言提供很强的表达能力。编程语言将与业务逻辑无关的底层实现隔离，以适应快速变化的需求。还需要编程语言提供检查程序正确性和可靠性的方案。另一个方面是性能，编程语言总是被期望具有较低的内存开销和时间开销，其主要依靠编译时（本地语言）和运行时（虚拟机语言）的优化。

| Evaluation Item | Meaning                                                      | Related Content                   |
| --------------- | ------------------------------------------------------------ | --------------------------------- |
| Expressiveness  | For abstract and complex business logic, programming languages can provide a concise way to describe it. | Programming Paradigm, Type System |
| Maintainability | After completing the business logic according to standard coding specifications, it is also easy to add new features or fix bugs subsequently. | Programming Paradigm, Type System |
| Reliability     | Non-crash under extreme conditions after completing business logic according to standard coding specifications. | Programming Paradigm, Type System |
| Performance     | Deploying a software system written in this programming language takes up fewer hardware resources when running on the target machine. | Time Overhead, Memory Overhead    |

但实际上，编程语言设计无法准确的进行定量分析。原因有两个方面。第一个是，某些评价的标准不是定量的。对于表达能力和可靠性，两者都是抽象的描述，它们在实践中有不可数种类的情况。如果想要对它们进行定量分析，那么必须将分析范围限制在某些具体的应用场景。第二个是，评价的标准不够充分。这里为了结构上的完整，放弃了一些难以测量的评价标准，如实时系统中的响应时间等。对于带有垃圾回收器的编程语言来说，垃圾回收带来的性能损耗不可忽略不计。尤其是基于JVM的编程语言，在进行垃圾回收时会有“Stop the World”的问题，这些都会影响编程语言的性能。但是这里为了简便起见不予考虑。

## 对编程范式的分析

编程范式是编程语言最底层的设计。从理论上来说，编程语言需要帮助人们完成对现实世界的建模。因此需要考虑应当提供哪些建模的基础设施，即是编程语言的编程范式。从实践上来说，设计编程语言应当首先考虑一等公民，作为编程语言的基本单位。

不同的编程范式之间存在一定的冲突，来源于一等公民的冲突。尤其是对已经确定的范式的编程语言增加新的范式，这是非常困难的。一个典型的例子是Java。Java8增加的函数式特性，并不能从根本上改变Java的编程范式，顶多算是语法糖。由于缺少最初的编程范式设计，Java难以完全兼容函数式特性。因此Java中的函数式是残缺的，不优雅的。因此，对于编程语言，尤其是对于多范式的现代编程语言，首先应当确定编程范式，进而确定一等公民。

函数式编程（FP）和面向对象编程（OOP）是现代编程语言中最常见，同时也是最重要的两种范式。因此，下文讨论编程范式时，选择这两种范式进行分析。	 


### 编程语言、范式和概念


![](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/Languages-paradigms-and-concepts.png)

```latex
\figure{Languages, paradigms, and concepts}\cite{van2009programming}
```

 该图`ref{fig:concept}`展示了语言，范式和概念之间的关系。每个范式包含一组核心的概念，同时，每个语言也实现了多种范式`\cite{van2009programming}`。FP和OOP是现代编程语言中通常具备的编程范式。而各个编程语言对它们的支持程度却并不相同，从表中可以看出各个语言对FP和OOP的概念的支持程度。

对于编程范式的概念，这里选择更加偏向于实践的组织方式。它将更偏向于选择具体编程语言中定义的概念，而不是编程范式理论中定义的概念。例如，对于函数式概念中的嵌套函数和匿名函数，它们在很多编程语言中被实现为lambda表达式。再比如，对于面向对象概念中的组合，目前流行的大多数编程语言都支持它。对于编程语言设计本身来说，组合仅仅是安排数据的一种方式，它并不属于任何一种范式。甚至像较为古老的语言——C语言，都提供了结构组合和函数组合。因此，并不将组合纳入分析。

| Paradigm | Concept                 | Meaning                                                      |
| -------- | ----------------------- | ------------------------------------------------------------ |
| FP       | Higher-order functions  | Take functions as variables, which can pass as parameters or return values. |
| FP       | Lambda expression       | A short block of code which takes in parameters and returns a value. |
| FP       | Partial application     | Given a function with certain parameters, producing another function (with fewer parameters). |
| FP       | Closures                | A record stores a function together with an environment`\cite{sussman1998scheme}`. |
| FP       | Type inference          | The automatic detection of the type of an expression at compile time. |
| FP       | Pattern matching        | Dispatch branch by matching the pattern of a given sequence of tokens. |
| FP       | Statement as expression | Take all statements as expressions, which creates a definite value. |
| OOP      | Encapsulation           | Hide the properties and implementation details of the object, and only expose the interface to the outside. |
| OOP      | Inheritance             | Create classes that are built upon existing classes`\cite{johnson1988designing}`. A way of code reuse. |
| OOP      | Composition             | Combine entities into more complex ones. A way of code reuse. |
| OOP      | Delegation              | One entity passing something to another entity`\cite{wilkinson2009grid}`. A way of code reuse. |
| OOP      | Traits                  | A set of method conventions. Broadly including trait, interface, protocol and mixin. |
| OOP      | Polymorphism            | Use of a single symbol to represent multiple different types`\cite{cardelli1985understanding}`. |
| OOP      | Everything is object    | All the basic elements of a programming language are represented in the form of objects. |

### Functional programming

FP最主要的特征就是其高度的抽象性。这主要是因为FP起源于lambda calculus，因此其具备了抽象代数的部分特征。在FP的实践中，主要思路是高阶函数的组合与嵌套，所以其设计核心就是理清高阶函数的嵌套与组合规则。因此，可以将其基本设计方法描述为：基于函数传递模型的标准化与高阶函数的可组合性，通过一系列的规则设计，完成数据从源头到结果的映射的描述。这里的映射是通过多个高阶函数的形式化组合完成的。描述就像数学公式，将输入的数据经过层层迭代，映射成结果。对于广义上数据的迭代过程，其中不仅仅包括数据本身的迭代，还包含规则的迭代。这是由于函数式语言中，数据与规则在语义上统一于函数。

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

随着编程语言实践的不断加深，编程语言对函数式概念的支持程度越来越高。对于上世纪八九十年代发布的编程语言，它们刚诞生时，对函数式概念的支持并不好。尤其是Java和C++。从最初的语法来看，人们设计它们时，并没有考虑到FP。而对于JavaScript和Python来说，它们在这方面好一些。它们天生支持一些函数式概念，但仍缺少了一些高级的函数式概念。至于2010年前后发布的编程语言，则都支持了基本的函数式概念。特别是Rust和Kotlin，还额外支持了“statements as expressions”的概念，这是MPL范式融合发展上的里程碑。“statements as expressions”这一概念在函数式语言中较为常见，它实现了表达式和语句语义上的统一。但大多数FP和OOP的范式融合的编程语言中都没有支持这一概念。随着编程语言的不断发展，Kotlin和Rust在支持此概念的基础上，实现了范式融合。因此可以将Kotlin和Rust视为具有更高层次的函数式范式。

### 面向对象编程

OOP最主要的特征就是强调复用。早在1960s，由于不断增加的软硬件复杂性，软件的维护变得愈发困难。OOP通过强调可复用性来解决这个问题。在OOP的实践中，人们将现实问题映射成实体及其关系，而不是关心处理问题的过程。面向对象语言诞生后，人们迫切需要从问题映射成实体及关系的建模方式，此时UML应运而生。它是一整套可视化的标准化建模语言，并极大的推动了面向对象方法学的发展。从此OOP的实践中，总是强调设计先于实现。

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


对于OOP的核心概念，流行语言的支持程度并没有太多的差距。由于其历史较为久远并且近些年来理论上和实践上并没有革命性的创新，因此大多数的编程语言已经相对完整地实现了OOP的核心概念。另外，越来越多的编程语言通过支持组合和委托来实现面向对象特性，而单纯的继承已经被证明是糟糕的实践。因此，Go和Rust中抛弃了类和继承的概念，通过trait来实现面向对象。相较于基于class的面向对象，基于trait的面向对象具有更松散的耦合和更灵活的实现。不过，大多数语言都同时支持trait和class，以实现不同粒度的控制。

为了更加准确地区分各个MPL的OOP概念支持程度，这里引入了一些并不常用的概念。一个例子是“everything is object”。根据OOP的定义，它本应当是OOP中最基本的概念。但事实上，早期的编程语言往往带有大量的指令式特征，即并不是将所有元素都看作对象。例如Java中仍然存在primitive data types，它们不是对象，因此我们不能像使用对象一样调用这些类型的方法。或许primitive data types有许多性能上的正面因素，但是从语义一致性的角度来讲，primitive data types是负面效果的。因此具有“everything is object”概念的语言被认为具有更好的面向对象特征。

### 编程范式与应用

由上面的论述可以得到该图，该图大致描述了不同编程语言对FP和OOP概念的支持程度。对于特定的编程语言，在图中越靠近FP/OOP，它对FP/OOP的支持程度就越好。实际上，编程语言的范式与其应用场景有非常密切的关系，无论是对单一应用领域的或是多种应用领域的语言。

![paradigm](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/paradigm.png)

一个典型的例子是Dart。它的主要应用场景是Web前端，它通常作为图形用户界面框架Flutter的支持语言。显然易见的是，我们使用的大多数图形用界面框架都有复杂的继承结构。这是因为，对于图形用户界面，它大多数应用场景都满足Liskov Substitution Principle，即子类型能够完全替换掉父亲类型。这是使用继承的充分条件。因此，Dart仅仅提供了基于继承的面向对象概念。

下个例子是Java，Java的FP和OOP概念支持都较弱。早年间，Java用于Enterprise的开发。它为了提高代码的复用性，提供了OOP。后来用于Web服务器，这个场景对业务逻辑的抽象有着很高的要求，因此Java额外增加了函数式的概念。但是因为历史遗留问题，支持的程度并不好。

还有一个例子是Kotlin，它对FP和OOP都有较好的支持。它最初的应用场景是安卓。为了解决以往Java开发安卓时太过繁琐的问题，Kotlin设计时就针对安卓的应用场景添加了大量FP和OOP的概念。其不仅支持传统的FP概念和OOP概念，还针对这些概念做了语法层面的优化，例如FP语法糖"trailing lambda"，和OOP委托语法糖"by"。这些有益的特性又使得Kotlin可以应用在其他场景，例如服务器前端和后端等。



## 对类型系统的分析

![type-definition](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/type-definition.png)

这里的讨论以L Cardelli的类型系统定义为基础`\cite{cardelli1996type}`。这种定义方式认为，类型系统的根本目的是要防止程序在运行过程中出现的错误，因此通过定义与错误相关的概念来定义类型系统。L. Cardelli认为，错误有两类。一种错误发生时会立刻导致fault，称之为trapped error。另一种不会立刻导致fault，称之为untrapped error。全部的untrapped error和部分trapped error构成了forbidden error。

首先通过定义何为错误来定义良好的行为。再根据编程语言行为检查的时机，划分为动态检查和静态检查。在静态检查的基础上，通过编程语言是否能检查出forbidden errors来划分强类型检查和弱类型检查。对于编程语言中的变量的类型，根据是否存在静态类型来限制运行时的变量类型范围，划分出有类型（静态）语言和无（静态）类型语言。再根据编程语言是否显式地指明变量的类型，划分出显式类型语言和隐式类型语言。

| Language   | Typed/Untyped | Explicitly/Implicitly typed | Dynamically/Staticly checked | Strongly/Weakly checked | Well behaved |
| ---------- | ------------- | --------------------------- | ---------------------------- | ----------------------- | ------------ |
| Python     | Typed         | Implicitly                  | Dynamically                  | Strongly                | Yes          |
| Java       | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |
| C++        | Typed         | Explicitly                  | Statically                   | Weakly                  | No           |
| JavaScript | Untyped       | -                           | Dynamically                  | -                       | Yes          |
| Go         | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |
| Swift      | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |
| Dart       | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |
| Rust       | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |
| Kotlin     | Typed         | Explicitly                  | Statically                   | Strongly                | Yes          |

一个常见的问题是，不同语境下的对编程语言类型系统的定义是不同的。一个典型的例子是JavaScript的类型系统。根据P Thiemann的分析，JavaScript是一个无类型的编程语言`\cite{thiemann2005towards}`。但是根据S Li的看法，JavaScript是一个动态类型语言`\cite{dot2015analysis}`。另一个值得讨论的事情是Python。Python在3.5版本引入了type hints。但type hints并没有实际上对Python进行错误检查。它仅仅是一种类型注释，来为静态语法分析程序提供更好的支持。Python错误检查的真正时机仍然是在运行时。

上表根据L Cardelli的类型系统定义，给出了常见MPLs的类型系统。值得注意的是，目前大多数MPL都具有一定的共性。它们常常具有显式类型、强类型、静态类型、行为良好的特点。这与这些语言的应用领域是分不开的。类型签名包含了约束信息，可以通过它们来间接判断变量或函数的作用。它提高了可维护性，这正是工业级应用的编程语言所需要的。静态类型检查和类型系统的形式化证明，可以提高程序的可靠性，帮助人们编写出错误更少的代码。并且静态的类型系统更有利于性能优化和内存分配，object program可以具备更好的性能。

## 对性能的分析

影响编程语言的性能的因素有很多，并不能单纯地认为某一种语言的性能高于另一种语言。编程语言的性能往往与应用场景密切相关。几种编程语言在不同的应用场景下可能会有截然不同的表现。为了准确地衡量编程语言的性能，引出了基准测试的概念。

基准测试提供了一种系统化测试同一种类下不同个体的性能差异的方法，为科学地判断个体优劣提供了基础`\cite{hockney1996science}`。一般在计算机科学中，基准测试指的是通过运行某个计算机程序或操作某些特定行为，用以评估对象的性能。它通常是通过一系列控制变量的对比实验通常，涉及几个迭代轮次，以便得出可复现的、精确的结论`\cite{fleming1986not}`。另外，它侧重于某一程序，应当排除无关程序对基准测试的影响。这要求基准测试应当清楚地知道底层的工作原理，避免系统的状态不确定性带来的误差。

### 基准测试设置

本次测试使用Python脚本统一进行粗粒度的批量测试，并使用内置的进程工具调用Linux系统命令，不依赖第三方库。这样做有较高的测试效率，因此适合大量数据。每一个测试程序都执行执行6次，对其结果取平均值以避免偏差。每个基准测试都执行了较大规模和较小规模的输入。所用测试平台的参数，见表`\ref{tab:platform}`。编程语言的编译参数，见表1。

对于同一种语言有不同的编译器或编译方式，对比不同的编译方式带来的差异（例如C++、Dart）。剩余的语言，按照编程语言类型系统和执行方式进行分类。因为对于多种相同类型系统和编译方式的语言，它们在实践中的应用范围应当是有重合的地方，因此对比分析。

| Language   | Version         | Dependency |
| ---------- | --------------- | ---------- |
| Python     | CPython3.8      |            |
| Java       | OpenJDK17       |            |
| C++        | Clang14/GCC11.2 |            |
| JavaScript | Node16          |            |
| Go         | Go1.17          |            |
| Swift      | Swift5.5        |            |
| Dart       | Dart2.16        |            |
| Rust       | Rust1.54        | MinGW7.3   |
| Kotlin     | Kotlin1.6       | OpenJDK8   |

计算机语言的基准测试度量指标，是出自于目前比较流行的跨语言基准测试套件之一——计算机语言基准测试游戏`\cite{gouy2017computer}`。对于每一种语言，有4个度量指标，分别是：


+ Compiler。标注在编程语言后，若无标注则使用官方编译器。
+ Size。用以表示源代码使用gzip压缩后的大小。对于同样的算法，某个编程语言使用的代码数量越少，则大致上可以认为该语言的语法表达能力越高。
+ CPU。运行该算法所需的时间。用多次运行的最小值表示。包括启动时间。
+ Memory。运行该算法峰值空间消耗。用多次运行的最大值表示。

### binary-trees

思路来源于Hans-J. Boehm的GC bench算法`\cite{demers1989combining}`。
通过反复大量分配、回收空间，测量的内存分配能力和垃圾回收能力。步骤如下：

![image-20220614193522405](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/image-20220614193522405.png)


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
kt-jvm    &  21 &      494 &    8.923 &  1783144 \\
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
kt-jvm    &  14 &      494 &   0.249 &    36040 \\
python3   &  14 &      589 &   0.911 &    14420 \\
rust      &  14 &      751 &   0.042 &     1176 \\
swift     &  14 &      714 &   0.299 &    17616 \\
\bottomrule
\end{tabular}
```

由表可以看出，Java的内存分配与管理速度在这几种编程语言中是最好的，即它的耗时极少。但是相对的，Java的内存消耗却是这几种语言中最大的。这与JVM独特的内存模型是分不开的。JVM将堆区域划分出不同的代，对于不同的代使用不同的垃圾回收算法。这样做的优点是显而易见的，它可以极大增加垃圾回收的效率。但与此同时，它却会占用多倍于实际需要的内存用以划分。由于Kotlin和Java都是基于JVM，所以它们有着相似的性能数据。但二者基于的JVM版本却有所不同。Kotlin基于的JVM版本是Java8，而Java基于的JVM版本是Java17。从Java9开始，JVM默认的GC是G1。相较于Java8的Parallel，G1是针对服务器GC，在大内存多核心的情况下，有着比Parallel更优秀的响应时间，但同时消耗了更多的内存。这与表中Kotlin的时间开销略高于Java、内存开销略低于Java的数据是应对的上的。

对于C++的两种不同编译器，Clang和GCC，它们的内存开销几乎相同。因为二者都是手动管理内存。但是，Clang却比GCC有着更低的时间消耗。这来自于编译器的优化。Clang的优化比GCC要好一些。二者的编译器架构是有所不同的，Clang-LLVM采用的是低耦合的前后端分离的架构，而GCC采用的是前后端耦合的架构，这是由于GCC诞生的年代相对古老一些，受限于时代背景。由此可见，对于本地编译型的语言，编译器的编译时优化占据较为重要的地位。

同为现代强类型、编译型语言的Rust、Go和Swift采用的是三种不同的内存管理模型。三者的测试结果与各自的内存模型有很大的关系。Go采用的是垃圾回收机制。虽然Go是编译型语言，但却有了额外的运行时以支持垃圾回收。同Java相比，Go采用的垃圾回收策略更为保守，并没有大刀阔斧地采用空间换时间的策略，使得其在内存分配中有不错的空间消耗和不太乐观的运行时间。而Rust采用的是所有权机制，不同于纯手动管理的C/C++，也不同于Go的垃圾回收。从底层实现上来说，Rust实际上并没有维护运行时来对空间进行管理，而是通过复杂的算有权算法在编译时确定回收内存的时机。因此，Rust的内存管理是没有开销的。事实上，Rust的编译器后端采用的是LLVM，和上述测试的Clang-LLVM组是同样的后端，从测试数据中也可以看出，Rust在浮点运算上与Cpp-Clang的性能几乎一样。但是由于Rust的所有权机制，在内存分配上，Rust更胜一筹。而对于Swift，其内存管理机制与上述都有所不同。Swift通过维护运行时的引用计数来进行内存分配和回收。此外，Swift强制原子地更新引用计数，而不是Rust在编译器对线程安全进行管理，这会导致高昂的CPU开销。由此可见，Swift相较于Rust有着更高的运行时开销，应证了Swift相较于Rust表现不佳的数据。

### n-body

思路来源于K. P. Rauch和D. P. Hamilton的Symplectic Integration算法`\cite{rauch2012hnbody}`。模拟多个行星的演进过程，并通过检查每个演进状态的能量来确认算法的正确性。
性能瓶颈主要集中在浮点运算。步骤如下：

![image-20220614193618815](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/image-20220614193618815.png)

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
kt-jvm    &  50000000 &     1124 &    6.914 &    37068 \\
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
kt-jvm    &  5000000 &     1124 &   0.826 &    37064 \\
python3   &  5000000 &     1196 &  52.530 &     7800 \\
rust      &  5000000 &     1480 &   0.579 &     1020 \\
swift     &  5000000 &     1192 &   0.976 &     6308 \\
\bottomrule
\end{tabular}
```

Java的时间消耗并不算很高，并没有落于本地编译语言太多。这归功于其独特的JIT机制。Java执行总时间较短的算法程序性能瓶颈主要在于JVM的启动时间和JIT优化的预热时间，而对于已经完成预热的算法，Java的执行速度丝毫不落后于本地编译型语言。Java代码的执行过程也比较有趣，它并不能用传统的解释执行或编译执行概括。Java的运行过程分为两步。第一步，源代码编译为字节码；第二步，JVM解释执行字节码。在解释执行字节码的过程中，JVM会受集代码的运行时信息，若发现某些字节码的执行频率较高，则会选择将此部分字节码进行编译，编译成本地代码，再次执行时直接调用本地代码。而Java所谓的性能优化主要在字节码，而源代码到字节码的阶段只进行了简单的优化。

JavaScript和Python有明显的性能差距。JavaScript在Google的V8引擎下，有着出色的性能表现。V8引擎执行js代码的过程和Java虚拟机执行字节码的过程很像，同样是采用了JIT优化的技术，所以相较于没有使用JIT优化的Python，它的性能有着巨大的提升，甚至和Java以及本地编译语言有着相差无几的浮点运算性能。事实上，大部分的应用场景下脚本语言的运行速度都不是性能瓶颈。但近年来JavaScript有最为“网页中的汇编”的趋势，越来越多的语言开始支持编译成JavaScript，例如Kotlin、Dart等，因此越来越多的业务逻辑需要通过JavaScript来执行，这就使得JavaScript负担了业务相关的繁琐逻辑，对于JavaScript的优化也是大势所趋。

### mandelbrot

在分辨率为N×N位图上，画出Mandelbrot Set。图像在复数轴上的坐标是[-1.5-i,0.5+i]。对于每个像素，进行一定次数的迭代，以确定当前像素的灰度。以PBF格式按照顺序逐byte输出。通过与标准输出对比以检查正确性。该测试的性能瓶颈在浮点运算、内存分配和IO。步骤如下：

![image-20220614193705439](C:/Users/dawn/Desktop/latex/iccs-latex/iccs.assets/image-20220614193705439.png)

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
kt-jvm    &  16000 &      407 &   30.032 &    28432 \\
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
kt-jvm    &  4000 &      407 &   2.419 &    28448 \\
python3   &  4000 &      688 &  46.273 &    12172 \\
rust      &  4000 &      868 &   0.757 &     4388 \\
swift     &  4000 &      394 &   1.661 &     6240 \\
\bottomrule
\end{tabular}
```

对于同样的Dart代码，采用的是JIT和AOT两种不同的方式编译。而对于时间开销和内存开销，两者有相似的表现。这是Dart的编译机制引起的。事实上，Dart的编译和运行机制与传统的方式都有所不同。传统的AOT是直接将程序代码编译为目标机上的目标代码，可以直接被操作系统调用、独立运行。但是对于Dart来说，无论是AOT或是JIT，仅仅只有编译时刻的不同，最终都是要在虚拟机上运行。实际上，当编译的时间可以忽略不计时，AOT与JIT的时间开销是大致相等的。但是这种运行方式有其独特的坏处，它会显著增加AOT模式的运行开销，但是这却使得AOT编译拥有了强大的运行时支持。正是因为这种特性，使得Dart虚拟机可以保存当前的运行状态，下次启动虚拟机的时候就可以直接加载上次的状态而不用重新启动。这样做主要是为了开发时的流畅体验，加之增量编译的特性，使之运行结果几乎随着代码的改动而实时变化，延迟极小。这样设计是因为Dart主要作为前端跨平台框架Flutter的开发语言被使用，使得对UI界面的改动可以实时反馈，而其他语言很难做到这一点。

单从影响的覆盖范围来看，编译方式对编程语言的效率影响是最大的。在工业应用中，多数场景下的性能瓶颈是编译相关的，这里编译指的是广义上的编译，不仅包括编译时，还包括运行时。编译对编程语言效率的影响又是多个方面的。第一，对于相同的编程语言，采用不同的编译器往往会有不同的目标代码，从而有不同的运行效率，这常常是因为编译时的优化效果不同，比如对于C++，Clang-LLVM的编译方式相对于采用GCC的编译方式得到的目标代码在运行时就有明显的性能差异。第二，对于相同的编程语言，不仅可以采取AOT（Ahead Of Time）的方式编译，还可以采用JIT（Just In Time）的方式编译，比如对于Kotlin，不仅可以编译成字节码运行在JVM，还可以编程成JavaScript代码运行在浏览器上，甚至可以采用Kotlin Native，编译成本地代码。这样以来，不同的编译方式对效率的影响就更大了。

内存管理虽然在日常使用时不显山露水，但是实际上内存管理对编程语言性能的影响是巨大的。第一，有关于堆上内存分配。对于带有虚拟机的语言，相较于非虚拟机语言，内存内存是有很大的优势的，这是因为虚拟机语言往往会提供内存池，对内存分配进行托管，而非虚拟机语言则没有这样的优势。当然，对于频繁使用堆上内存的场景下，非虚拟机语言往往会使用自定义的或第三方提供的内存池框架，工业中的性能差距并没有这么明显。同样，对于虚拟机语言来说，托管的内存池也并非全是好事，当发生内存抖动时，虚拟机会频繁地进行GC，这也影响了性能。第二，有关于内存的局部性原理。若是顺序访问内存，CPU会将相邻的数据都放入Cache中，大大提高了Cache的命中率，从而提高了性能。同时配合编译器的循环展开优化，则会达到更高的效率。若是对内存进行随机访问，则性能就要大打折扣。

### 总结

当前，相较于PLT主流的研究方向，对编程语言设计的分析仍是较为稀少的。尤其是仍缺少系统化研究编程语言设计的方法。本文则是通过影响编程语言应用的几个方面，借助几种特定MPLs，从应用场景对编程语言设计进行了相对全面的分析。

第二章我们讨论了MPL的定义。根据MPL的定义，筛选出IEEE Spectrum上的具有MPL特征的流行编程语言，并给出了良好的编程语言设计应当考虑的因素。第三章我们站在了编程范式的角度，给出了判断MPL编程范式支持程度的方法，并对被选择的MPLs支持程度做了分析。之后探讨了编程范式支持强弱和编程语言应用之间关系。第四章我们讨论了类型系统。根据L Cardelli的类型系统定义，讨论了被选择的MPLs的类型系统。第五章我们通过三个基准测试，分别从内存分配、浮点运算、IO，讨论了编程语言的性能。然后，站在编程语言实现的角度，结合这些语言的应用场景，分析基准测试的数据，解释了这些MPLs性能表现的原因。

编程范式、类型系统和性能，这个三个相互联系的部分，贯穿了编程语言从设计到实现的多个需要考虑的方面。而这三个分支本身，每个对编程语言设计的影响更是举足轻重，影响了编程语言表达能力、可维护性、可靠性和性能，这几个编程语言最重要的指标。事实上我们可以得到，编程语言的理论和实践发展过程总是动态的，编程语言的设计和实现也是动态的。本文动态地从现代编程语言角度谈论编程语言的设计，这一点会是编程语言设计与分析角度比较好的研究思路。
