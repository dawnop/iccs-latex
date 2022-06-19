# Analysis of the Design of Several Modern Programming Languages

## Introduction

In programming language theory(PLT), programming language design has always been an important topic. Because programming language design integrates various branches of PLT, it is the prerequisite for the final realization of programming language. By analyzing the design of modern programming languages(MPL), we can obtain the trend of programming language development, i.e., the programming language design can be fed back from the application point of view.

By and large, the overall amount of literature on the analysis of programming language design is relatively small. M Coblenz argues that programming language design should be considered in terms of several theories related to programming languages and gives a way of evaluating programming language design. A Stefika gives some issues to consider in programming language design and argues that type systems are crucial for programming languages. However, the above results focus on a qualitative analysis of programming language design and lack some concrete examples. LA Meyerovich analyzes the usage of popular programming languages to give best practices in programming language design through statistics. This analytical approach is somewhat lacking in theoretical and systematic aspects. F Morandat provides a systematic analysis of the R language, including performance, syntactic design, and application scenarios. The article adopts a better research approach and can be used to broaden the analysis perspective based on that article. Currently, there is a lack of a systematic, theoretical and practical, wide-ranging, application-oriented analysis of programming language design.

By analyzing the design of MPL, this paper draws the influence of different programming language factors on the application. e.g., as programming languages evolve, why does the level of paradigm support in MPLs keep changing, what type systems are in MPLs, how MPLs keep programs efficient, and so on.

## Several modern programming languages

There is no uniform definition for MPL. Many researchers define it in different ways, such as finding connections between software engineering and the development of MPLs or elaborating around abstractions. The above are correct in their respective fields, and here the definition is abstracted and extended to adapt to more domains. By studying the intersection of different definitions, a more general definition of MPLs derives. It is an obvious fact, that as the practice of programming languages continues to deepen, so does the definition of MPL. This is because PLT is a field where practice and theory go hand in hand. Although its definition is constantly changing, some core elements remain the same. Here are a few features of MPLs.

1. Substance over form. It should provide descriptive grammatical structures, rather than hand-in-hand telling the machine what to do. This item emphasizes the abstraction of machine functions.
2. Semantic Consistency. For similar grammatical structures, there should be similar grammatical functions.
3. Syntax bootstrapping. For non-core grammatical features, based on following semantic consistency, they should be composed of core grammatical features.
4. Paradigm fusion. Multiple programming paradigms should be provided without forcing users to use a particular programming paradigm.

### Select modern programming languages to be analyzed

For the selection of programming language, the first thing is to pay attention to the most popular trend at the moment, so the selected programming language must cover the most commonly used. Secondly, there are many classification standards for programming languages. For each division standard, the selected programming language should cover most of the options. Then, it should focus on highlighting the new programming language with excellent design in recent years. These programming languages have absorbed the advantages of past programming languages and improved their inherent shortcomings. From these selected MPLs, we can see the development trend of application-oriented programming languages over the years.

According to the data of the *IEEE spectrum*, the top five most popular programming languages in 2021 are Python, Java, C, C ++, and JavaScript. An exception case is C, which provides low-level access to computer systems. To be precise, the syntax design of the C language does not conform to any of the features of MPL. And for the four remaining popular programming languages mentioned above, they are still helpful even though they are not exactly in line with the features of MPL. In the era when these languages were first created, they emerged as representatives of MPLs. Therefore these four past programming languages are compared with the emerging MPLs.

Based on the above-mentioned MPL features, several programming languages were selected and arranged in order of popularity, resulting in Go, Swift, Dart, Rust and Kotlin (where Swift and Dart are equally popular). Coincidentally, these languages are also released in this order sequentially. In addition, all of these languages have similar type systems and all support AOT compilation, and although not all use garbage collection memory management, they all move away from manual memory management. These are all common features of MPLs in today's application environment.

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

### How to evaluate the design of programming languages

The evaluation criteria here are more application-oriented, to evaluate whether a programming language is "practical" or not. Rather than evaluating whether a programming language is "elegant" as in the features of MPL. In fact, most of the popular languages are not elegant. This is because the development of programming languages is not just a technical matter, but involves political, commercial, and other external factors. A typical example is the evolution of JavaScript. Today, the language standards for JavaScript in mainstream browsers are still not uniform. But this does not prevent JavaScript from being the most popular language for Web applications. But that doesn't stop JavaScript from being the most useful language for Web applications. There are two main aspects that determine whether a language is useful or not. One aspect is its syntactic design. For complex business logic, programming languages are required to provide strong expressiveness, i.e., isolate underlying implementations that are not related to the business logic to accommodate rapid changes. Programming languages are also needed to provide solutions for checking the correctness and reliability of programs. Another aspect is performance, where programming languages are always expected to have a low memory overhead and time overhead, relying heavily on compile-time (native languages) and run-time (virtual machine languages) optimizations.

In practice, however, programming language design cannot be accurately quantified. The reason for this is twofold. The first is that certain criteria for evaluation are not quantitative. For expressiveness and reliability, both are abstract descriptions, and they have uncountable kinds of cases in practice. If one wants to analyze them quantitatively, then one must restrict the analysis to certain specific application scenarios. The second is that the evaluation criteria are not sufficient. Some evaluation criteria that are difficult to measure, such as response time in a real-time system, are dropped here for structural completeness. For programming languages with garbage collectors, the performance loss from garbage collection is not negligible. In particular, JVM-based programming languages have the problem of "Stop the World" when garbage collection is performed, which affects the performance of the programming language. However, this is not considered for the sake of simplicity.

## Programming paradigm

Programming paradigms are derived from the way people perceive the real world, and programming languages are tools for formally describing this way of perception.

When people develop software, they model the real world in a computer. The different programming paradigms lie in the different ways of modeling the real world.

Modeling the world often requires solving two problems.

The first is to find out what the basic unit of the world is and call it a first-class citizen.

These first-class citizens can be stored in variables and can be passed as function parameters and function return values.

In short, it is the "data" to be manipulated in the program. 

The second is to find the relationships between these basic units, called calculations.

Calculation can be seen as "operation" of data in the program.
