# Introduction to Compilers

# and Language Design

## Second Edition

## Prof. Douglas Thain

## University of Notre Dame


**Introduction to Compilers and Language Design**
Copyright © 2020 Douglas Thain.
Paperback ISBN: 979-8-655-18026-
Second edition.

Anyone is free to download and print the PDF edition of this book for per-
sonal use. Commercial distribution, printing, or reproduction without the
author’s consent is expressly prohibited. All other rights are reserved.

You can find the latest version of the PDF edition, and purchase inexpen-
sive hardcover copies athttp://compilerbook.org

Revision Date: January 28, 2022


```
iii
```
For Lisa, William, Zachary, Emily, and Alia.

```
iii
```

iv

```
iv
```

```
v
```
## Contributions

I am grateful to the following people for their contributions to this book:
Andrew Litteken drafted the chapter on ARM assembly; Kevin Latimer
drew the RegEx to NFA and the LR example figures; Benjamin Gunning
fixed an error in LL(1) parse table construction; Tim Shaffer completed the
detailed LR(1) example.
And the following people corrected typos:
Sakib Haque (27), John Westhoff (26), Emily Strout (26), Gonzalo Martinez
(25), Daniel Kerrigan (24), Brian DuSell (23), Ryan Mackey (20), TJ Dasso
(18), Nedim Mininovic (15), Noah Yoshida (14), Joseph Kimlinger (12),
Nolan McShea (11), Jongsuh Lee (11), Kyle Weingartner (10), Andrew Lit-
teken (9), Thomas Cane (9), Samuel Battalio (9), Stephane Massou (8), Luis ́
Prieb (7), William Diederich (7), Jonathan Xu (6), Gavin Inglis (6), Kath-
leen Capella (6), Edward Atkinson (6), Tanner Juedeman (5), John Johnson
(4), Luke Siela (4), Francis Schickel (4), Eamon Marmion (3), Molly Zachlin
(3), David Chiang (3), Jacob Mazur (3), Spencer King (2), Yaoxian Qu (2),
Maria Aranguren (2), Patrick Lacher (2), Connor Higgins (2), Tango Gu (2),
Andrew Syrmakesis (2), Horst von Brand (2), John Fox (2), Jamie Zhang
(2), Benjamin Gunning (1) Charles Osborne (1), William Theisen (1), Jes-
sica Cioffi (1), Ben Tovar (1), Ryan Michalec (1), Patrick Flynn (1), Clint
Jeffery (1), Ralph Siemsen (1), John Quinn (1), Paul Brunts (1), Luke Wurl
(1), Bruce Mardle (1), Dane Williams (1), Thomas Fisher (1), Alan Johnson
(1), Jacob Harris (1), Jeff Clinton (1), Reto Habluetzel (1), Chris Fietkiewicz
(1)
Please send any comments or corrections via email to Prof. Douglas
Thain (dthain@nd.edu).

```
v
```

vi

```
vi
```

## CONTENTS vii






xii CONTENTS

```
xii
```

## LIST OF FIGURES xiii


- 1 Introduction Contents
   - 1.1 What is a compiler?
   - 1.2 Why should you study compilers?
   - 1.3 What’s the best way to learn about compilers?
   - 1.4 What language should I use?
   - 1.5 How is this book different from others?
   - 1.6 What other books should I read?
- 2 A Quick Tour
   - 2.1 The Compiler Toolchain
   - 2.2 Stages Within a Compiler
   - 2.3 Example Compilation
   - 2.4 Exercises
- 3 Scanning
   - 3.1 Kinds of Tokens
   - 3.2 A Hand-Made Scanner
   - 3.3 Regular Expressions
   - 3.4 Finite Automata
      - 3.4.1 Deterministic Finite Automata
      - 3.4.2 Nondeterministic Finite Automata
   - 3.5 Conversion Algorithms
      - 3.5.1 Converting REs to NFAs
      - 3.5.2 Converting NFAs to DFAs
      - 3.5.3 Minimizing DFAs
   - 3.6 Limits of Finite Automata
   - 3.7 Using a Scanner Generator
   - 3.8 Practical Considerations
   - 3.9 Exercises
   - 3.10 Further Reading
- 4 Parsing
   - 4.1 Overview
   - 4.2 Context Free Grammars
      - 4.2.1 Deriving Sentences viii CONTENTS
      - 4.2.2 Ambiguous Grammars
   - 4.3 LL Grammars
      - 4.3.1 Eliminating Left Recursion
      - 4.3.2 Eliminating Common Left Prefixes
      - 4.3.3 First and Follow Sets
      - 4.3.4 Recursive Descent Parsing
      - 4.3.5 Table Driven Parsing
   - 4.4 LR Grammars
      - 4.4.1 Shift-Reduce Parsing
      - 4.4.2 The LR(0) Automaton
      - 4.4.3 SLR Parsing
      - 4.4.4 LR(1) Parsing
      - 4.4.5 LALR Parsing
   - 4.5 Grammar Classes Revisited
   - 4.6 The Chomsky Hierarchy
   - 4.7 Exercises
   - 4.8 Further Reading
- 5 Parsing in Practice
   - 5.1 The Bison Parser Generator
   - 5.2 Expression Validator
   - 5.3 Expression Interpreter
   - 5.4 Expression Trees
   - 5.5 Exercises
   - 5.6 Further Reading
- 6 The Abstract Syntax Tree
   - 6.1 Overview
   - 6.2 Declarations
   - 6.3 Statements
   - 6.4 Expressions
   - 6.5 Types
   - 6.6 Putting it All Together
   - 6.7 Building the AST
   - 6.8 Exercises
- 7 Semantic Analysis
   - 7.1 Overview of Type Systems
   - 7.2 Designing a Type System
   - 7.3 The B-Minor Type System
   - 7.4 The Symbol Table
   - 7.5 Name Resolution
   - 7.6 Implementing Type Checking
   - 7.7 Error Messages
   - 7.8 Exercises CONTENTS ix
   - 7.9 Further Reading
- 8 Intermediate Representations
   - 8.1 Introduction
   - 8.2 Abstract Syntax Tree
   - 8.3 Directed Acyclic Graph
   - 8.4 Control Flow Graph
   - 8.5 Static Single Assignment Form
   - 8.6 Linear IR
   - 8.7 Stack Machine IR
   - 8.8 Examples
      - 8.8.1 GIMPLE - GNU Simple Representation
      - 8.8.2 LLVM - Low Level Virtual Machine
      - 8.8.3 JVM - Java Virtual Machine
   - 8.9 Exercises
   - 8.10 Further Reading
- 9 Memory Organization
   - 9.1 Introduction
   - 9.2 Logical Segmentation
   - 9.3 Heap Management
   - 9.4 Stack Management
      - 9.4.1 Stack Calling Convention
      - 9.4.2 Register Calling Convention
   - 9.5 Locating Data
   - 9.6 Program Loading
   - 9.7 Further Reading
- 10 Assembly Language
   - 10.1 Introduction
   - 10.2 Open Source Assembler Tools
   - 10.3 X86 Assembly Language
      - 10.3.1 Registers and Data Types
      - 10.3.2 Addressing Modes
      - 10.3.3 Basic Arithmetic
      - 10.3.4 Comparisons and Jumps
      - 10.3.5 The Stack
      - 10.3.6 Calling a Function
      - 10.3.7 Defining a Leaf Function
      - 10.3.8 Defining a Complex Function
   - 10.4 ARM Assembly
      - 10.4.1 Registers and Data Types
      - 10.4.2 Addressing Modes
      - 10.4.3 Basic Arithmetic
      - 10.4.4 Comparisons and Branches x CONTENTS
      - 10.4.5 The Stack
      - 10.4.6 Calling a Function
      - 10.4.7 Defining a Leaf Function
      - 10.4.8 Defining a Complex Function
      - 10.4.9 64-bit Differences
   - 10.5 Further Reading
- 11 Code Generation
   - 11.1 Introduction
   - 11.2 Supporting Functions
   - 11.3 Generating Expressions
   - 11.4 Generating Statements
   - 11.5 Conditional Expressions
   - 11.6 Generating Declarations
   - 11.7 Exercises
- 12 Optimization
   - 12.1 Overview
   - 12.2 Optimization in Perspective
   - 12.3 High Level Optimizations
      - 12.3.1 Constant Folding
      - 12.3.2 Strength Reduction
      - 12.3.3 Loop Unrolling
      - 12.3.4 Code Hoisting
      - 12.3.5 Function Inlining
      - 12.3.6 Dead Code Detection and Elimination
   - 12.4 Low-Level Optimizations
      - 12.4.1 Peephole Optimizations
      - 12.4.2 Instruction Selection
   - 12.5 Register Allocation
      - 12.5.1 Safety of Register Allocation
      - 12.5.2 Priority of Register Allocation
      - 12.5.3 Conflicts Between Variables
      - 12.5.4 Global Register Allocation
   - 12.6 Optimization Pitfalls
   - 12.7 Optimization Interactions
   - 12.8 Exercises
   - 12.9 Further Reading
- A Sample Course Project
   - A.1 Scanner Assignment
   - A.2 Parser Assignment
   - A.3 Pretty-Printer Assignment
   - A.4 Typechecker Assignment
   - A.5 Optional: Intermediate Representation CONTENTS xi
   - A.6 Code Generator Assignment
   - A.7 Optional: Extend the Language
- B The B-Minor Language
   - B.1 Overview
   - B.2 Tokens
   - B.3 Types
   - B.4 Expressions
   - B.5 Declarations and Statements
   - B.6 Functions
   - B.7 Optional Elements
- C Coding Conventions
- Index
- 2.1 A Typical Compiler Toolchain List of Figures
- 2.2 The Stages of a Unix Compiler
- 2.3 Example AST
- 2.4 Example Intermediate Representation
- 2.5 Example Assembly Code
- 3.1 A Simple Hand Made Scanner
- 3.2 Relationship Between REs, NFAs, and DFAs
- 3.3 Subset Construction Algorithm
- 3.4 Converting an NFA to a DFA via Subset Construction
- 3.5 Hopcroft’s DFA Minimization Algorithm
- 3.6 Structure of a Flex File
- 3.7 Example Flex Specification
- 3.8 Example Main Program
- 3.9 Example Token Enumeration
- 3.10 Build Procedure for a Flex Program
- 4.1 Two Derivations of the Same Sentence
- 4.2 A Recursive-Descent Parser
- 4.3 LR(0) Automaton for GrammarG
- 4.4 SLR Parse Table for GrammarG
- 4.5 Part of LR(0) Automaton for GrammarG
- 4.6 LR(1) Automaton for GrammarG
- 4.7 The Chomsky Hierarchy
- 5.1 Bison Specification for Expression Validator
- 5.2 Main Program for Expression Validator
- 5.3 Build Procedure for Bison and Flex Together
- 5.4 Bison Specification for an Interpreter
- 5.5 AST for Expression Interpreter
- 5.6 Building an AST for the Expression Grammar
- 5.7 Evaluating Expressions
- 5.8 Printing and Evaluating Expressions
- 7.1 The Symbol Structure
- 7.2 A Nested Symbol Table xiv LIST OF FIGURES
- 7.3 Symbol Table API
- 7.4 Name Resolution for Declarations
- 7.5 Name Resolution for Expressions
- 8.1 Sample DAG Data Structure Definition
- 8.2 Example of Constant Folding
- 8.3 Example Control Flow Graph
- 9.1 Flat Memory Model
- 9.2 Logical Segments
- 9.3 Multiprogrammed Memory Layout
- 10.1 X86 Register Structure
- 10.2 X86 Register Structure
- 10.3 Summary of System V ABI Calling Convention
- 10.4 System V ABI Register Assignments
- 10.5 Example X86-64 Stack Layout
- 10.6 Complete X86 Example
- 10.7 ARM Addressing Modes
- 10.8 ARM Branch Instructions
- 10.9 Summary of ARM Calling Convention
- 10.10ARM Register Assignments
- 10.11Example ARM Stack Frame
- 10.12Complete ARM Example
- 11.1 Code Generation Functions
- 11.2 Example of Generating X86 Code from a DAG
- 11.3 Expression Generation Skeleton
- 11.4 Generating Code for a Function Call
- 11.5 Statement Generator Skeleton
- 12.1 Timing a Fast Operation
- 12.2 Constant Folding Pseudo-Code
- 12.3 Example X86 Instruction Templates
- 12.4 Example of Tree Rewriting
- 12.5 Live Ranges and Register Conflict Graph
- 12.6 Example of Global Register Allocation


##### 1

## Chapter 1 – Introduction

### 1.1 What is a compiler?

A **compiler** translates a program in a **source language** to a program in
a **target language**. The most well known form of a compiler is one that
translates a high level language like C into the native assembly language
of a machine so that it can be executed. And of course there are compilers
for other languages like C++, Java, C#, and Rust, and many others.

The same techniques used in a traditional compiler are also used in
any kind of program that processes a language. For example, a typeset-
ting program like TEX translates a manuscript into a Postscript document.
A graph-layout program like Dot consumes a list of nodes and edges and
arranges them on a screen. A web browser translates an HTML document
into an interactive graphical display. To write programs like these, you
need to understand and use the same techniques as in traditional compil-
ers.

Compilers exist not only totranslateprograms, but also toimprovethem.
A compiler assists a programmer by finding errors in a program at compile
time, so that the user does not have to encounter them at runtime. Usually,
a more strict language results in more compile-time errors. This makes the
programmer’s job harder, but makes it more likely that the program is
correct. For example, the Ada language is infamous among programmers
as challenging to write without compile-time errors, but once working, is
trusted to run safety-critical systems such as the Boeing 777 aircraft.

A compiler is distinct from an **interpreter** , which reads in a program
and then executes it directly, without emitting a translation. This is also
sometimes known as a **virtual machine**. Languages like Python and Ruby
are typically executed by an interpreter that reads the source code directly.

Compilers and interpreters are closely related, and it is sometimes pos-
sible to exchange one for the other. For example, Java compilers translate
Java source code into Java **bytecode** , which is an abstract form of assem-
bly language. Some implementations of the Java Virtual Machine work as
interpreters that execute one instruction at a time. Others work by trans-
lating the bytecode into local machine code, and then running the machine
code directly. This is known as **just in time compiling** or **JIT**.


##### 2 CHAPTER 1. INTRODUCTION

### 1.2 Why should you study compilers?

You will be a better programmer. A great craftsman must understand his
or her tools, and a programmer is no different. By understanding more
deeply how a compiler translates your program into machine language,
you will become more skilled at writing effective code and debugging it
when things go wrong.
You can create tools for debugging and translating.If you can write a parser
for a given language, then you can write all manner of supporting tools
that help you (and others) debug your own programs. An integrated de-
velopment environment like Eclipse incorporates parsers for languages
like Java, so that it can highlight syntax, find errors without compiling,
and connect code to documentation as you write.
You can create new languages. A surprising number of problems are
made easier by expressing them compactly in a custom language. (These
are sometimes known as **domain specific languages** or simply **little lan-
guages** .) By learning the techniques of compilers, you will be able to im-
plement little languages and avoid some pitfalls of language design.
You can contribute to existing compilers.While it’s unlikely that you will
write the next great C compiler (since we already have several), language
and compiler development does not stand still. Standards development
results in new language features; optimization research creates new ways
of improving programs; new microprocessors are created; new operating
systems are developed; and so on. All of these developments require the
continuous improvement of existing compilers.
You will have fun while solving challenging problems.Isn’t that enough?

### 1.3 What’s the best way to learn about compilers?

The best way to learn about compilers is towrite your own compilerfrom
beginning to end. While that may sound daunting at first, you will find
that this complex task can be broken down into several stages of moder-
ate complexity. The typical undergraduate computer science student can
write a complete compiler for a simple language in a semester, broken
down into four or five independent stages.

### 1.4 What language should I use?

Without question, you should use the C programming language and the
X86 assembly language, of course!
Ok, maybe the answer isn’t quite that simple. There is an ever-increasing
number of programming languages that all have different strengths and
weaknesses. Java is simple, consistent, and portable, albeit not high per-
formance. Python is easy to learn and has great library support, but is
weakly typed. Rust offers exceptional static type-safety, but is not (yet)


##### 1.5. HOW IS THIS BOOK DIFFERENT FROM OTHERS? 3

widely used. It is quite possible to write a compiler in nearly any lan-
guage, and you could use this book as a guide to do so.
However, we really think that you should learn C, write a compiler in
C, and use it to compile a C-like language which produces assembly for a
widely-used processor, like X86 or ARM. Why? Because it is important for
you to learn the ins and outs of technologies that are in wide use, and not
just those that are abstractly beautiful.
C is the most widely-used portable language for low-level coding (com-
pilers, and libraries, and kernels) and it is also small enough that one
can learn how to compile every aspect of C in a single semester. True, C
presents some challenges related to type safety and pointer use, but these
are manageable for a project the size of a compiler. There are other lan-
guages with different virtues, but none as simple and as widely used as C.
Once you write a C compiler, then you are free to design your own (better)
language.
Likewise, the X86 has been the most widely-deployed computer archi-
tecture in desktops, servers, and laptops for several decades. While it is
considerably more complex than other architectures like MIPS or SPARC
or ARM, one can quickly learn the essential subset of instructions nec-
essary to build a compiler. Of course, ARM is quickly catching up as a
popular architecture in the mobile, embedded, and low power space, so
we have included a section on that as well.
That said, the principles presented in this book are widely applicable.
If you are using this as part of a class, your instructor may very well choose
a different compilation language and different target assembly, and that’s
fine too.

### 1.5 How is this book different from others?

Most books on compilers are very heavy on the abstract theory of scan-
ners, parsers, type systems, and register allocation, and rather light on
how the design of a language affects the compiler and the runtime. Most
are designed for use by a graduate survey of optimization techniques.
This book takes a broader approach by giving a lighter dose of opti-
mization, and introducing more material on the process of engineering a
compiler, the tradeoffs in language design, and considerations for inter-
pretation and translation.
You will also notice that this book doesn’t contain a whole bunch of
fiddly paper-and-pencil assignments to test your knowledge of compiler
algorithms. (Ok, there are a few of those in Chapters 3 and 4.) If you want
to test your knowledge, then write some working code. To that end, the
exercises at the end of each chapter ask you to take the ideas in the chapter,
and either explore some existing compilers, or write parts of your own. If
you do all of them in order, you will end up with a working compiler,
summarized in the final appendix.


##### 4 CHAPTER 1. INTRODUCTION

### 1.6 What other books should I read?

For general reference on compilers, I suggest the following books:

- **Charles N. Fischer, Ron K. Cytron, and Richard J. LeBlanc Jr, “Craft-**
    **ing a Compiler”, Pearson, 2009.**
    This is an excellent undergraduate textbook which focuses on object-oriented soft-
    ware engineering techniques for constructing a compiler, with a focus on generating
    output for the Java Virtual Machine.
- **Christopher Fraser and David Hanson, “A Retargetable C Com-**
    **piler: Design and Implementation”, Benjamin/Cummings, 1995.**
    Also known as the “LCC book”, this book focuses entirely on explaining the C imple-
    mentation of a C compiler by taking the unusual approach of embedding the literal
    code into the textbook, so that code and explanation are intertwined.
- **Alfred V. Aho, Monica S. Lam, Ravi Sethi, and Jeffrey D. Ullman,**
    **“Compilers: Principles, Techniques, and Tools”, Addison Wesley,**
    **2006.** Affectionately known as the “dragon book”, this is a comprehensive treat-
    ment of the theory of compilers from scanning through type theory and optimization
    at an advanced graduate level.

```
Ok, what are you waiting for? Let’s get to work.
```

##### 5

## Chapter 2 – A Quick Tour

### 2.1 The Compiler Toolchain

A compiler is one component in a **toolchain** of programs used to create
executables from source code. Typically, when you invoke a single com-
mand to compile a program, a whole sequence of programs are invoked in
the background. Figure 2.1 shows the programs typically used in a Unix
system for compiling C source code to assembly code.

```
Preprocessor
(cpp)
Preprocessed
Source
Compiler
(cc1)
Assembly
(prog.s)
Assembler
(as)
```
```
Object Code
(prog.o)
Static
Linker
(ld)
```
```
Executable
(prog)
```
```
Libraries
(libc.a)
```
```
Dynamic
Linker
(ld.so)
```
```
Running
Process
```
```
Dynamic Libraries
(libc.so)
```
```
Source
(prog.c)
```
```
Headers
(stdio.h)
```
```
Figure 2.1: A Typical Compiler Toolchain
```
- The **preprocessor** prepares the source code for the compiler proper.
    In the C and C++ languages, this means consuming all directives that
    start with the#symbol. For example, an#includedirective causes
    the preprocessor to open the named file and insert its contents into
    the source code. A#definedirective causes the preprocessor to
    substitute a value wherever a macro name is encountered. (Not all
    languages rely on a preprocessor.)
- The **compiler** proper consumes the clean output of the preproces-
    sor. It scans and parses the source code, performs typechecking and


##### 6 CHAPTER 2. A QUICK TOUR

```
other semantic routines, optimizes the code, and then produces as-
sembly language as the output. This part of the toolchain is the main
focus of this book.
```
- The **assembler** consumes the assembly code and produces **object**
    **code**. Object code is “almost executable” in that it contains raw ma-
    chine language instructions in the form needed by the CPU. How-
    ever, object code does not know the final memory addresses in which
    it will be loaded, and so it contains gaps that must be filled in by the
    linker.
- The **linker** consumes one or more object files and library files and
    combines them into a complete, executable program. It selects the
    final memory locations where each piece of code and data will be
    loaded, and then “links” them together by writing in the missing ad-
    dress information. For example, an object file that calls theprintf
    function does not initially know the address of the function. An
    empty (zero) address will be left where the address must be used.
    Once the linker selects the memory location ofprintf, it must go
    back and write in the address at every place whereprintfis called.

In Unix-like operating systems, the preprocessor, compiler, assembler,
and linker are historically namedcpp,cc1,as, andldrespectively. The
user-visible programccsimply invokes each element of the toolchain in
order to produce the final executable.

### 2.2 Stages Within a Compiler

In this book, our focus will be primarily on the compiler proper, which is
the most interesting component in the toolchain. The compiler itself can
be divided into several stages:

```
Scanner Tokens Parser AbstractSyntaxTree SemanticRoutines RepresentationIntermediate
```
```
Optimizers
```
```
CharacterStream GeneratorCode AssemblyCode
```
```
Figure 2.2: The Stages of a Unix Compiler
```
- The **scanner** consumes the plain text of a program, and groups to-
    gether individual characters to form complete **tokens**. This is much
    like grouping characters into words in a natural language.


##### 2.3. EXAMPLE COMPILATION 7

- The **parser** consumes tokens and groups them together into com-
    plete statements and expressions, much like words are grouped into
    sentences in a natural language. The parser is guided by a **grammar**
    which states the formal rules of composition in a given language.
    The output of the parser is an **abstract syntax tree (AST)** that cap-
    tures the grammatical structures of the program. The AST also re-
    members where in the source file each construct appeared, so it is
    able to generate targeted error messages, if needed.
- The **semantic routines** traverse the AST and derive additional mean-
    ing (semantics) about the program from the rules of the language
    and the relationship between elements of the program. For exam-
    ple, we might determine thatx + 10is afloatexpression by ob-
    serving the type ofxfrom an earlier declaration, then applying the
    language rule that addition betweenintandfloatvalues yields
    a float. After the semantic routines, the AST is often converted into
    an **intermediate representation (IR)** which is a simplified form of
    assembly code suitable for detailed analysis. There are many forms
    of IR which we will discuss in Chapter 8.
- One or more **optimizers** can be applied to the intermediate represen-
    tation, in order to make the program smaller, faster, or more efficient.
    Typically, each optimizer reads the program in IR format, and then
    emits the same IR format, so that each optimizer can be applied in-
    dependently, in arbitrary order.
- Finally, a **code generator** consumes the optimized IR and transforms
    it into a concrete assembly language program. Typically, a code gen-
    erator must perform **register allocation** to effectively manage the
    limited number of hardware registers, and **instruction selection** and
    **sequencing** to order assembly instructions in the most efficient form.

### 2.3 Example Compilation

Suppose we wish to compile this fragment of code into assembly:

height = (width+56) * factor(foo);

The first stage of the compiler (the scanner) will read in the text of
the source code character by character, identify the boundaries between
symbols, and emit a series of **tokens**. Each token is a small data structure
that describes the nature and contents of each symbol:

```
id:height = ( id:width + int:56 ) * id:factor ( id:foo ) ;
```
At this stage, the purpose of each token is not yet clear. For example,
factorandfooare simply known to be identifiers, even though one is


##### 8 CHAPTER 2. A QUICK TOUR

the name of a function, and the other is the name of a variable. Likewise,
we do not yet know the type ofwidth, so the+could potentially rep-
resent integer addition, floating point addition, string concatenation, or
something else entirely.
The next step is to determine whether this sequence of tokens forms
a valid program. The parser does this by looking for patterns that match
the **grammar** of a language. Suppose that our compiler understands a
language with the following grammar:

```
Grammar G 1
```
1. expr→expr + expr
2. expr→expr * expr
3. expr→expr = expr
4. expr→id ( expr )
5. expr→( expr )
6. expr→id
7. expr→int

Each line of the grammar is called a rule, and explains how various
parts of the language are constructed. Rules 1-3 indicate that an expression
can be formed by joining two expressions with operators. Rule 4 describes
a function call. Rule 5 describes the use of parentheses. Finally, rules 6 and
7 indicate that identifiers and integers are atomic expressions.^1
The parser looks for sequences of tokens that can be replaced by the
left side of a rule in our grammar. Each time a rule is applied, the parser
creates a node in a tree, and connects the sub-expressions into the **abstract
syntax tree (AST)**. The AST shows the structural relationships between
each symbol: addition is performed onwidthand 56 , while a function
call is applied tofactorandfoo.
With this data structure in place, we are now prepared to analyze the
meaning of the program. The **semantic routines** traverse the AST and de-
rive additional meaning by relating parts of the program to each other, and
to the definition of the programming language. An important component
of this process is **typechecking** , in which the type of each expression is
determined, and checked for consistency with the rest of the program. To
keep things simple here, we will assume that all of our variables are plain
integers.
To generate linear intermediate code, we perform a post-order traver-
sal of the AST and generate an IR instruction for each node in the tree. A
typical IR looks like an abstract assembly language, with load/store in-
structions, arithmetic operations, and an infinite number of registers. For
example, this is a possible IR representation of our example program:

(^1) The careful reader will note that this example grammar has ambiguities. We will discuss
that in some detail in Chapter 4.


##### 2.3. EXAMPLE COMPILATION 9

##### ASSIGN

##### ID

```
height MUL
```
##### ADD CALL

##### ID

```
width
```
##### INT

##### 56

##### ID

```
factor
```
##### ID

```
foo
```
```
Figure 2.3: Example AST
```
```
LOAD $56 -> r1
LOAD width -> r2
IADD r1, r2 -> r3
ARG foo
CALL factor -> r4
IMUL r3, r4 -> r5
STOR r5 -> height
```
```
Figure 2.4: Example Intermediate Representation
```
The intermediate representation is where most forms of optimization
occur. Dead code is removed, common operations are combined, and code
is generally simplified to consume fewer resources and run more quickly.

Finally, the intermediate code must be converted to the desired assem-
bly code. Figure 2.5 shows X86 assembly code that is one possible trans-
lation of the IR given above. Note that the assembly instructions do not
necessarily correspond one-to-one with IR instructions.
A well-engineered compiler is highly modular, so that common code
elements can be shared and combined as needed. To support multiple lan-
guages, a compiler can provide distinct scanners and parsers, each emit-
ting the same intermediate representation. Different optimization tech-
niques can be implemented as independent modules (each reading and


##### 10 CHAPTER 2. A QUICK TOUR

MOVQ width, %rax # load width into rax
ADDQ $56, %rax # add 56 to rax
MOVQ %rax, -8(%rbp) # save sum in temporary
MOVQ foo, %edi # load foo into arg 0 register
CALL factor # invoke factor, result in rax
MOVQ -8(%rbp), %rbx # load sum into rbx
IMULQ %rbx # multiply rbx by rax
MOVQ %rax, height # store result into height

```
Figure 2.5: Example Assembly Code
```
writing the same IR) so that they can be enabled and disabled indepen-
dently. A retargetable compiler contains multiple code generators, so that
the same IR can be emitted for a variety of microprocessors.

### 2.4 Exercises

1. Determine how to invoke the preprocessor, compiler, assembler, and
    linker manually in your local computing environment. Compile a
    small complete program that computes a simple expression, and ex-
    amine the output at each stage. Are you able to follow the flow of
    the program in each form?
2. Determine how to change the optimization level for your local com-
    piler. Find a non-trivial source program and compile it at multiple
    levels of optimization. How does the compile time, program size,
    and run time vary with optimization levels?
3. Search the internet for the formal grammars for three languages that
    you are familiar with, such as C++, Ruby, and Rust. Compare them
    side by side. Which language is inherently more complex? Do they
    share any common structures?


##### 11

## Chapter 3 – Scanning

### 3.1 Kinds of Tokens

Scanning is the process of identifying **tokens** from the raw text source code
of a program. At first glance, scanning might seem trivial – after all, iden-
tifying words in a natural language is as simple as looking for spaces be-
tween letters. However, identifying tokens in source code requires the
language designer to clarify many fine details, so that it is clear what is
permitted and what is not.
Most languages will have tokens in these categories:

- **Keywords** are words in the language structure itself, likewhileor
    classortrue. Keywords must be chosen carefully to reflect the
    natural structure of the language, without interfering with the likely
    names of variables and other identifiers.
- **Identifiers** are the names of variables, functions, classes, and other
    code elements chosen by the programmer. Typically, identifiers are
    arbitrary sequences of letters and possibly numbers. Some languages
    require identifiers to be marked with a **sentinel** (like the dollar sign
    in Perl) to clearly distinguish identifiers from keywords.
- **Numbers** could be formatted as integers, or floating point values, or
    fractions, or in alternate bases such as binary, octal or hexadecimal.
    Each format should be clearly distinguished, so that the programmer
    does not confuse one with the other.
- **Strings** are literal character sequences that must be clearly distin-
    guished from keywords or identifiers. Strings are typically quoted
    with single or double quotes, but also must have some facility for
    containing quotations, newlines, and unprintable characters.
- **Comments** and **whitespace** are used to format a program to make it
    visually clear, and in some cases (like Python) are significant to the
    structure of a program.
When designing a new language, or designing a compiler for an exist-
ing language, the first job is to state precisely what characters are permit-
ted in each type of token. Initially, this could be done informally by stating,


##### 12 CHAPTER 3. SCANNING

token_t scan_token( FILE *fp ) {
int c = fgetc(fp);
if(c==’*’) {
return TOKEN_MULTIPLY;
} else if(c==’!’) {
char d = fgetc(fp);
if(d==’=’) {
return TOKEN_NOT_EQUAL;
} else {
ungetc(d,fp);
return TOKEN_NOT;
}
} else if(isalpha(c)) {
do {
char d = fgetc(fp);
} while(isalnum(d));
ungetc(d,fp);
return TOKEN_IDENTIFIER;
} else if (... ) {

}
}

```
Figure 3.1: A Simple Hand Made Scanner
```
for example,“An identifier consists of a letter followed by any number of letters
and numerals.”, and then assigning a symbolic constant (TOKENIDENTIFIER)
for that kind of token. As we will see, an informal approach is often am-
biguous, and a more rigorous approach is needed.

### 3.2 A Hand-Made Scanner

Figure 3.1 shows how one might write a scanner by hand, using simple
coding techniques. To keep things simple, we only consider just a few
tokens:*for multiplication,!for logical-not,!=for not-equal, and se-
quences of letters and numbers for identifiers.
The basic approach is to read one character at a time from the input
stream (fgetc(fp)) and then classify it. Some single-character tokens are
easy: if the scanner reads a * character, it immediately returns
TOKENMULTIPLY, and the same would be true for addition, subtraction,
and so forth.
However, some characters are part of multiple tokens. If the scanner
encounters!, that could represent a logical-not operation by itself, or it
could be the first character in the!=sequence representing not-equal-to.


##### 3.3. REGULAR EXPRESSIONS 13

Upon reading!, the scanner must immediately read the next character. If
the next character is=, then it has matched the sequence!=and returns
TOKENNOTEQUAL.
But, if the character following!is something else, then the non-matching
character needs to beput backon the input stream usingungetc, because
it is not part of the current token. The scanner returnsTOKENNOTand will
consume the put-back character on the next call toscantoken.
In a similar way, once a letter has been identified byisalpha(c), then
the scanner keeps reading letters or numbers, until a non-matching char-
acter is found. The non-matching character is put back, and the scanner
returnsTOKENIDENTIFIER.
(We will see this pattern come up in every stage of the compiler: an
unexpected item doesn’t match the current objective, so it must be put
back for later. This is known more generally as **backtracking** .)
As you can see, a hand-made scanner is rather verbose. As more to-
ken types are added, the code can become quite convoluted, particularly
if tokens share common sequences of characters. It can also be difficult
for a developer to be certain that the scanner code corresponds to the de-
sired definition of each token, which can result in unexpected behavior on
complex inputs. That said, for a small language with a limited number of
tokens, a hand-made scanner can be an appropriate solution.
For a complex language with a large number of tokens, we need a more
formalized approach to defining and scanning tokens. A formal approach
will allow us to have a greater confidence that token definitions do not
conflict and the scanner is implemented correctly. Further, a formal ap-
proach will allow us to make the scanner compact and high performance

- surprisingly, the scanner itself can be the performance bottleneck in a
compiler, since every single character must be individually considered.
    The formal tools of **regular expressions** and **finite automata** allow us
to state very precisely what may appear in a given token type. Then, auto-
mated tools can process these definitions, find errors or ambiguities, and
produce compact, high performance code.

### 3.3 Regular Expressions

Regular expressions (REs) are a language for expressing patterns. They
were first described in the 1950s by Stephen Kleene[4]as an element of
his foundational work in automata theory and computability. Today, REs
are found in slightly different forms in programming languages (Perl),
standard libraries (PCRE), text editors (vi), command-line tools (grep),
and many other places. We can use regular expressions as a compact
and formal way of specifying the tokens accepted by the scanner of a
compiler, and then automatically translate those expressions into working
code. While easily explained, REs can be a bit tricky to use, and require
some practice in order to achieve the desired results.


##### 14 CHAPTER 3. SCANNING

```
Let us define regular expressions precisely:
```
```
A regular expression sis a string which denotesL(s), a set of strings
drawn from an alphabetΣ. L(s)is known as the “language ofs.”
```
```
L(s)is defined inductively with the following base cases:
```
- Ifa∈Σthenais a regular expression andL(a) ={a}.
- is a regular expression andL()contains only the empty string.

```
Then, for any regular expressionssandt:
```
1. s|tis a RE such thatL(s|t) =L(s)∪L(t).
2. stis a RE such thatL(st)contains all strings formed by the
    concatenation of a string inL(s)followed by a string inL(t).
3. s∗is a RE such thatL(s∗) =L(s)concatenated zero or more times.

```
Rule #3 is known as the Kleene closure and has the highest precedence.
Rule #2 is known as concatenation. Rule #1 has the lowest precedence and
is known as alternation. Parentheses can be added to adjust the order of
operations in the usual way.
```
Here are a few examples using just the basic rules. (Note that a finite
RE can indicate an infinite set.)
**Regular Expression s Language L(s)**
hello { hello }
d(o|i)g { dog,dig }
moo* { mo,moo,mooo,... }
(moo)* { ,moo,moomoo,moomoomoo,... }
a(b|a)*a { aa,aaa,aba,aaaa,aaba,abaa,... }
The syntax described so far is entirely sufficient to write any regular
expression. But, it is also handy to have a few helper operations built on
top of the basic syntax:

```
s?indicates thatsis optional.
s?can be written as(s|)
```
```
s+indicates thatsis repeated one or more times.
s+can be written asss*
```
```
[a-z]indicates any character in that range.
[a-z]can be written as(a|b|...|z)
```
```
[ˆx]indicates any character except one.
[ˆx]can be written asΣ- x
```

##### 3.4. FINITE AUTOMATA 15

Regular expressions also obey several algebraic properties, which make
it possible to re-arrange them as needed for efficiency or clarity:

```
Associativity: a|(b|c) = (a|b)|c
Commutativity: a|b = b|a
Distribution: a(b|c) = ab|ac
Idempotency: a** = a*
```
Using regular expressions, we can precisely state what is permitted in
a given token. Suppose we have a hypothetical programming language
with the following informal definitions and regular expressions. For each
token type, we show examples of strings that match (and do not match)
the regular expression.
Informal definition: An identifier is a sequence of capital letters and num-
bers, but a number must not come first.
Regular expression: [A-Z]+([A-Z]|[0-9])*
Matches strings: PRINT
MODE5
Does not match: hello
4YOU
Informal definition: A number is a sequence of digits with an optional dec-
imal point. For clarity, the decimal point must have
digits on both left and right sides.
Regular expression: [0-9]+(.[0-9]+)?
Matches strings: 123
3.14
Does not match: .15
30.
Informal definition: A comment is any text (except a right angle bracket)
surrounded by angle brackets.
Regular expression: <[ˆ>]*>
Matches strings: <tricky part>
<<<<look left>
Does not match: <this is an <illegal> comment>

### 3.4 Finite Automata

A **finite automaton (FA)** is an abstract machine that can be used to repre-
sent certain forms of computation. Graphically, an FA consists of a number
of states (represented by numbered circles) and a number of edges (repre-
sented by labeled arrows) between those states. Each edge is labeled with
one or more symbols drawn from an alphabetΣ.
The machine begins in a start stateS 0. For each input symbol presented
to the FA, it moves to the state indicated by the edge with the same label


##### 16 CHAPTER 3. SCANNING

as the input symbol. Some states of the FA are known as **accepting states**
and are indicated by a double circle. If the FA is in an accepting state after
all input is consumed, then we say that the FA **accepts** the input. We say
that the FA **rejects** the input string if it ends in a non-accepting state, or if
there is no edge corresponding to the current input symbol.
Every RE can be written as an FA, and vice versa. For a simple regular
expression, one can construct an FA by hand. For example, here is an FA
for the keywordfor:

##### 0 1 3

```
f
2
o r
```
```
Here is an FA for identifiers of the form[a-z][a-z0-9]+
```
##### 0 2

```
a-z
0-9
```
##### 1

```
a-z
```
```
a-z
0-9
```
```
And here is an FA for numbers of the form([1-9][0-9]*)|0
```
##### 0

##### 1 0-9 2

##### 0-9

##### 3

##### 1-9

##### 0

#### 3.4.1 Deterministic Finite Automata

Each of these three examples is a **deterministic finite automaton** (DFA).
A DFA is a special case of an FA where every state has no more than one
outgoing edge for a given symbol. Put another way, a DFA has no am-
biguity: for every combination of state and input symbol, there is exactly
one choice of what to do next.
Because of this property, a DFA is very easy to implement in software
or hardware. One integer (c) is needed to keep track of the current state.


##### 3.4. FINITE AUTOMATA 17

The transitions between states are represented by a matrix (M[s,i]) which
encodes the next state, given the current state and input symbol. (If the
transition is not allowed, we mark it withEto indicate an error.) For each
symbol, we computec=M[s,i]until all the input is consumed, or an error
state is reached.

#### 3.4.2 Nondeterministic Finite Automata

The alternative to a DFA is a **nondeterministic finite automaton (NFA)**.
An NFA is a perfectly valid FA, but it has an ambiguity that makes it some-
what more difficult to work with.

Consider the regular expression[a-z]*ing, which represents all lower-
case words ending in the suffixing. It can be represented with the follow-
ing automaton:

##### 0 3

```
[a-z]
```
##### 1

```
i
2
n g
```
Now consider how this automaton would consume the wordsing. It
could proceed in two different ways. One would be to move to state 0 on
s, state 1 oni, state 2 onn, and state 3 ong. But the other, equally valid
way would be to stay in state 0 the whole time, matching each letter to the
[a-z]transition. Both ways obey the transition rules, but one results in
acceptance, while the other results in rejection.

The problem here is that state 0 allows for two different transitions on
the symboli. One is to stay in state 0 matching[a-z]and the other is to
move to state 1 matchingi.

Moreover, there is no simple rule by which we can pick one path or
another. If the input issing, the right solution is to proceed immediately
from state zero to state one oni. But if the input issinging, then we
should stay in state zero for the firstingand proceed to state one for the
seconding.

An NFA can also have an(epsilon) transition, which represents the
empty string. This transition can be taken without consuming any input
symbols at all. For example, we could represent the regular expression
a*(ab|ac)with this NFA:


##### 18 CHAPTER 3. SCANNING

##### 0

##### 3

##### 6

```
a
ε^1
```
##### 4

```
ε
```
```
a 2
b
```
```
a 5 c
```
This particular NFA presents a variety of ambiguous choices. From
state zero, it could consumeaand stay in state zero. Or, it could take an
to state one or state four, and then consume anaeither way.
There are two common ways to interpret this ambiguity:

- The **crystal ball interpretation** suggests that the NFA somehow “knows”
    what the best choice is, by some means external to the NFA itself. In
    the example above, the NFA would choose whether to proceed to
    state zero, one, or four before consuming the first character, and it
    would always make the right choice. Needless to say, this isn’t pos-
    sible in a real implementation.
- The **many-worlds interpretation** suggests that the NFA exists in all
    allowable statessimultaneously. When the input is complete, if any
    of those states are accepting states, then the NFA has accepted the
    input. This interpretation is more useful for constructing a working
    NFA, or converting it to a DFA.

Let us use the many-worlds interpretation on the example above. Sup-
pose that the input string isaaac. Initially the NFA is in state zero. With-
out consuming any input, it could take an epsilon transition to states one
or four. So, we can consider its initial state to be all of those states si-
multaneously. Continuing on, the NFA would traverse these states until
accepting the complete stringaaac:

```
States Action
0, 1, 4 consumea
0, 1, 2, 4, 5 consumea
0, 1, 2, 4, 5 consumea
0, 1, 2, 4, 5 consumec
6 accept
```
In principle, one can implement an NFA in software or hardware by
simply keeping track of all of the possible states. But this is inefficient.
In the worst case, we would need to evaluate all states for all characters
on each input transition. A better approach is to convert the NFA into an
equivalent DFA, as we show below.


##### 3.5. CONVERSION ALGORITHMS 19

### 3.5 Conversion Algorithms

Regular expressions and finite automata are all equally powerful. For ev-
ery RE, there is an FA, and vice versa. However, a DFA is by far the most
straightforward of the three to implement in software. In this section, we
will show how to convert an RE into an NFA, then an NFA into a DFA,
and then to optimize the size of the DFA.

```
Regular
Expression
```
```
Nondeterministic
Finite
Automaton
```
```
Thompson's
Construction DeterministicFinite
Automaton
```
```
Subset
Construction Code
Transition
Matrix
```
```
Figure 3.2: Relationship Between REs, NFAs, and DFAs
```
#### 3.5.1 Converting REs to NFAs

To convert a regular expression to a nondeterministic finite automaton, we
can follow an algorithm given first by McNaughton and Yamada[5], and
then by Ken Thompson[6].
We follow the same inductive definition of regular expression as given
earlier. First, we define automata corresponding to the base cases of REs:

```
The NFA for any characterais: The NFA for antransition is:
```
```
a ε
```
Now, suppose that we have already constructed NFAs for the regular
expressionsAandB, indicated below by rectangles. BothAandBhave
a single start state (on the left) and accepting state (on the right). If we
write the concatenation ofAandBasAB, then the corresponding NFA is
simplyAandBconnected by antransition. The start state ofAbecomes
the start state of the combination, and the accepting state ofBbecomes the
accepting state of the combination:

```
The NFA for the concatenationABis:
```
```
A ε B
```

##### 20 CHAPTER 3. SCANNING

In a similar fashion, the alternation ofAandBwritten asA|Bcan be ex-
pressed as two automata joined by common starting and accepting nodes,
all connected bytransitions:

```
The NFA for the alternationA|Bis:
```
```
ε
ε
```
```
A
```
```
B
```
```
ε
ε
```
Finally, the Kleene closureA*is constructed by taking the automaton
forA, adding starting and accepting nodes, then addingtransitions to
allow zero or more repetitions:

```
The NFA for the Kleene closureA*is:
```
```
ε
ε
```
```
A
ε
```
```
ε
```
**Example.** Let’s consider the process for an example regular expression
a(cat|cow)*. First, we start with the innermost expressioncatand as-
semble it into three transitions resulting in an accepting state. Then, do the
same thing forcow, yielding these two FAs:

```
c a t
```
```
c o w
```
The alternation of the two expressionscat|cowis accomplished by
adding a new starting and accepting node, with epsilon transitions. (The
boxes are not part of the graph, but simply highlight the previous graph
components carried forward.)


##### 3.5. CONVERSION ALGORITHMS 21

```
ε
ε
```
```
c
```
```
c
```
```
ε
ε
```
```
a t
```
```
o w
```
Then, the Kleene closure(cat|cow)*is accomplished by adding an-
other starting and accepting state around the previous FA, with epsilon
transitions between:

```
ε
```
```
ε
```
```
ε
```
```
ε
```
```
c ε
```
```
c
```
```
ε
```
```
ε
```
```
a t
```
```
o w
```
Finally, the concatenation ofa(cat|cow)*is achieved by adding a
single state at the beginning fora:

```
a ε ε
```
```
ε
```
```
ε
```
```
ε
ε
c
```
```
c
```
```
ε
```
```
ε
a t
```
```
o w
```
You can easily see that the NFA resulting from the construction algo-
rithm, while correct, is quite complex and contains a large number of ep-
silon transitions. An NFA representing the tokens for a complete language
could end up having thousands of states, which would be very impractical
to implement. Instead, we can convert this NFA into an equivalent DFA.


##### 22 CHAPTER 3. SCANNING

#### 3.5.2 Converting NFAs to DFAs

We can convert any NFA into an equivalent DFA using the technique of
**subset construction**. The basic idea is to create a DFA such that each state
in the DFA corresponds to multiple states in the NFA, according to the
“many-worlds” interpretation.
Suppose that we begin with an NFA consisting of statesNand start
stateN 0. We wish to construct an equivalent DFA consisting of statesD
and start stateD 0. EachDstate will correspond to multipleNstates. First,
we define a helper function known as the **epsilon closure** :

**Epsilon closure.**
−closure(n)is the set of NFA states reachable from NFA statenby zero
or moretransitions.
Now we define the subset construction algorithm. First, we create a
start stateD 0 corresponding to the−closure(N 0 ). Then, for each outgo-
ing charactercfrom the states inD 0 , we create a new state containing the
epsilon closure of the states reachable byc. More precisely:

```
Subset Construction Algorithm.
Given an NFA with statesNand start stateN 0 , create an equivalent DFA
with statesDand start stateD 0.
```
```
LetD 0 =−closure(N 0 ).
AddD 0 to a list.
While items remain on the list:
Letdbe the next DFA state removed from the list.
For each charactercinΣ:
LetTcontain all NFA statesNksuch that:
Nj∈dandNj
c
−→Nk
Create new DFA stateDi=−closure(T)
IfDiis not already in the list, add it to the end.
```
```
Figure 3.3: Subset Construction Algorithm
```

##### 3.5. CONVERSION ALGORITHMS 23

```
N0 a N1 ε N2 ε N3 N13
```
```
ε
```
```
ε N8
```
```
N4
```
```
ε N12
```
```
N9 ε
c
```
```
c N5
```
```
N11 ε
```
```
N7
```
```
ε
```
```
a N10 t
```
```
o N6 w
```
```
D0:
N0
```
```
D1:
N1, N2, N3,
N4, N8, N13
```
```
a D2:
N5, N9
```
```
c
```
```
D3:
N6
o
```
```
D5:
N10
```
```
a
```
```
D4:
N7, N12, N13,
N2, N3, N4, N8
```
```
w
```
```
c
```
```
D6:
N11, N12, N13,
t N2,N3, N4, N8
```
```
c
```
```
Figure 3.4: Converting an NFA to a DFA via Subset Construction
```
**Example.** Let’s work out the algorithm on the NFA in Figure 3.4. This
is the same NFA corresponding to the REa(cat|cow)*with each of the
states numbered for clarity.

1. ComputeD 0 which is−closure(N 0 ). N 0 has notransitions, so
    D 0 ={N 0 }. AddD 0 to the work list.
2. RemoveD 0 from the work list. The characterais an outgoing tran-
    sition fromN 0 toN 1 .−closure(N 1 ) ={N 1 ,N 2 ,N 3 ,N 4 ,N 8 ,N 13 }so
    add all of those to new stateD 1 and addD 1 to the work list.
3. RemoveD 1 from the work list. We can see thatN 4 −→c N 5 andN 8 −→c
    N 9 , so we create a new stateD 2 ={N 5 ,N 9 }and add it to the work
    list.
4. RemoveD 2 from the work list. Bothaandoare possible transitions
    because ofN 5 −→o N 6 andN 9 −→a N 10. So, create a new stateD 3 for the
    otransition toN 6 and new stateD 5 for theatransition toN 10. Add
    bothD 3 andD 5 to the work list.
5. RemoveD 3 from the work list. The only possible transition isN 6
    w
−→
    N 7 so create a new stateD 4 containing the−closure(N 7 )and add it
    to the work list.
6. RemoveD 5 from the work list. The only possible transition isN 10
    t
    −→
    N 11 so create a new stateD 6 containing−closure(N 11 )and add it to
    the work list.


##### 24 CHAPTER 3. SCANNING

7. RemoveD 4 from the work list, and observe that the only outgoing
    transitioncleads to statesN 5 andN 9 which already exist as stateD 2 ,
    so simply add a transitionD 4 −→c D 2.
8. RemoveD 6 from the work list and, in a similar way, addD 6
    c
−→D 2.
9. The work list is empty, so we are done.

#### 3.5.3 Minimizing DFAs

The subset construction algorithm will definitely generate a valid DFA,
but the DFA may possibly be very large (especially if we began with a
complex NFA generated from an RE.) A large DFA will have a large tran-
sition matrix that will consume a lot of memory. If it doesn’t fit in L1 cache,
the scanner could run very slowly. To address this problem, we can apply
Hopcroft’s algorithm to shrink a DFA into a smaller (but equivalent) DFA.
The general approach of the algorithm is to optimistically group to-
gether all possibly-equivalent statesSinto super-statesT. Initially, we
place all non-acceptingSstates into super-stateT 0 and accepting states
into super-stateT 1. Then, we examine the outgoing edges in each state
s∈Ti. If, a given characterchas edges that begin inTiand end indif-
ferentsuper-states, then we consider the super-state to beinconsistentwith
respect toc. (Consider an impermissible transition as if it were a transi-
tion toTE, a super-state for errors.) The super-state must then be split into
multiple states that are consistent with respect toc. Repeat this process for
all super-states and all charactersc∈Σuntil no more splits are required.

```
DFA Minimization Algorithm.
Given a DFA with statesS, create an equivalent DFA with
an equal or fewer number of statesT.
```
```
First partitionSintoTsuch that:
T 0 =non-accepting states ofS.
T 1 =accepting states ofS.
Repeat:
∀Ti∈T:
∀c∈Σ:
ifTi−→{c more than oneTstate},
then splitTiinto multipleTstates
such thatchas the same action in each.
Until no more states are split.
```
```
Figure 3.5: Hopcroft’s DFA Minimization Algorithm
```

##### 3.5. CONVERSION ALGORITHMS 25

**Example.** Suppose we have the following non-optimized DFA and
wish to reduce it to a smaller DFA:

```
1
```
```
2
```
```
a
```
```
b^3
```
```
a
```
```
4
```
```
b
```
```
b
```
```
a
a b^5
a
```
```
b
```
We begin by grouping all of non-accepting states 1, 2, 3, 4 into one
super-state and the accepting state 5 into another super-state, like this:

```
1,2,3,4 5
```
```
b
a
b
```
Now, we ask whether this graph is consistent with respect to all possi-
ble inputs, by referring back to the original DFA. For example, we observe
that, if we are in super-state (1,2,3,4) then an input ofaalways goes to
state 2, which keeps us within the super-state. So, this DFA is consistent
with respect toa. However, from super-state (1,2,3,4) an input ofbcan
either stay within the super-state or go to super-state (5). So, the DFA is
inconsistent with respect tob.
To fix this, we try splitting out one of the inconsistent states (4) into a
new super-state, taking the transitions with it:

```
1,2,3
```
```
b 4 b 5
a
a,b
```
Again, we examine each super-state for consistency with respect to
each input character. Again, we observe that super-state 1,2,3 is consis-
tent with respect toa, but not consistent with respect tobbecause it can
either lead to state 3 or state 4. We attempt to fix this by splitting out state
2 into its own super-state, yielding this DFA.


##### 26 CHAPTER 3. SCANNING

```
1,3
```
```
b
```
```
a 2
```
```
a
```
```
b 4
a
```
```
b 5
```
```
b
```
```
a
```
Again, we examine each super-state and observe that each possible in-
put is consistent with respect to the super-state, and therefore we have the
minimal DFA.

### 3.6 Limits of Finite Automata

Regular expressions and finite automata are powerful and effective at rec-
ognizing simple patterns in individual words or tokens, but they are not
sufficient to analyze all of the structures in a problem. For example, could
you use a finite automaton to match an arbitrary number of nested paren-
theses?
It’s not hard to write out an FA that could match, say, up to three pairs
of nested parentheses, like this:

```
0 ( 1
)
```
```
( 2
)
```
```
( 3
)
```
But the key word isarbitrary! To match any number of parentheses
would require an infinite automaton, which is obviously impractical. Even
if we were to apply some practical upper limit (say, 100 pairs) the automa-
ton would still be impractically large when combined with all the other
elements of a language that must be supported.
For example, a language like Python permits the nesting of parentheses
()for precedence, curly brackets{}to represent dictionaries, and square
brackets[]to represent lists. An automaton to match up to 100 nested
pairs of each in arbitrary order would have 1,000,000 states!
So, we limit ourselves to using regular expressions and finite automata
for the narrow purpose of identifying the words and symbols within a
problem. To understand the higher level structure of a program, we will
instead use parsing techniques introduced in Chapter 4.

### 3.7 Using a Scanner Generator

Because a regular expression precisely describes all the allowable forms
of a token, we can use a program to automatically transform a set of reg-


##### 3.7. USING A SCANNER GENERATOR 27

##### %{

```
(C Preamble Code)
%}
(Character Classes)
%%
(Regular Expression Rules)
%%
(Additional Code)
```
```
Figure 3.6: Structure of a Flex File
```
ular expressions into code for a scanner. Such a program is known as a
**scanner generator**. The programLex, developed at AT&T, was one of the
earliest examples of a scanner generator. Vern Paxson translatedLexinto
the C language to createFlex, which is distributed under the Berkeley li-
cense and is widely used in Unix-like operating systems today to generate
scanners implemented in C or C++.
To use Flex, we write a specification of the scanner that is a mixture of
regular expressions, fragments of C code, and some specialized directives.
The Flex program itself consumes the specification and produces regular
C code that can then be compiled in the normal way.
Figure 3.6 gives the overall structure of a Flex file. The first section con-
sists of arbitrary C code that will be placed at the beginning ofscanner.c,
like include files, type definitions, and similar things. Typically, this is
used to include a file that contains the symbolic constants for tokens.
The second section declares character classes, which are symbolic short-
hands for commonly used regular expressions. For example, you might
declareDIGIT [0-9]. This class can be referred to later as{DIGIT}.
The third section is the most important part. It states a regular expres-
sion for each type of token that you wish to match, followed by a fragment
of C code that will be executed whenever the expression is matched. In the
simplest case, this code returns the type of the token, but it can also be used
to extract token values, display errors, or anything else appropriate.
The fourth section is arbitrary C code that will go at the end of the
scanner, typically for additional helper functions. A peculiar requirement
of Flex is that we must define a functionyywrap()which returns 1 to
indicate that the input is complete at the end of the file. If we wanted to
continue scanning in another file, thenyywrap()would open the next file
and return 0.
The regular expression language accepted by Flex is very similar to
that of formal regular expressions discussed above. The main difference is
that characters that have special meaning with a regular expression (like
parentheses, square brackets, and asterisks) must be escaped with a back-
slash or surrounded with double quotes. Also, a period (.) can be used to


##### 28 CHAPTER 3. SCANNING

match any character at all, which is helpful for catching error conditions.
Figure 3.7 shows a simple but complete example to get you started.
This specification describes just a few tokens: a single character addition
(which must be escaped with a backslash), thewhilekeyword, an iden-
tifier consisting of one or more letters, and a number consisting of one or
more digits. As is typical in a scanner, any other type of character is an
error, and returns an explicit token type for that purpose.
Flex generates the scanner code, but not a complete program, so you
must write amainfunction to go with it. Figure 3.8 shows a simple driver
program that uses this scanner. First, the main program must declare as
externthe symbols it expects to use in the generated scanner code:yyin
is the file from which text will be read,yylexis the function that imple-
ments the scanner, and the arrayyytextcontains the actual text of each
token discovered. Finally, we must have a consistent definition of the to-
ken types across the parts of the program, so intotoken.hwe put an
enumeration describing the new typetokent. This file is included in
bothscanner.flexandmain.c.
Figure 3.10 shows how all the pieces come together.scanner.flexis
converted into scanner.c by invoking flex -o scanner.c
scanner.flex. Then, bothmain.candscanner.care compiled to
produce object files, which are linked together to produce the complete
program.

### 3.8 Practical Considerations

**Handling keywords.** In many languages, keywords (such aswhileor
if) would otherwise match the definitions of identifiers, unless specially
handled. There are several solutions to this problem. One is to enter a
regular expression for every single keyword into the Flex specification.
(These must precede the definition of identifiers, since Flex will accept the
first expression that matches.) Another is to maintain a single regular ex-
pression that matches all identifiers and keywords. The action associated
with that rule can compare the token text with a separate list of keywords
and return the appropriate type. Yet another approach is to treat all key-
words and identifiers as a single token type, and allow the problem to be
sorted out by the parser. (This is necessary in languages like PL/1, where
identifiers can have the same names as keywords, and are distinguished
by context.)
**Tracking source locations.** In later stages of the compiler, it is useful
for the parser or typechecker to know exactly what line and column num-
ber a token was located at, usually to print out a helpful error message.
(“Undefined symbol spider at line 153.”) This is easily done by having
the scanner match newline characters, and increase the line count (but not
return a token) each time one is found.
**Cleaning tokens.** Strings, characters, and similar token types need to


##### 3.8. PRACTICAL CONSIDERATIONS 29

**Contents of File: scanner.flex**

%{
#include "token.h"
%}
DIGIT [0-9]
LETTER [a-zA-Z]
%%
(" "|\t|\n) /* skip whitespace*/
\+ { return TOKEN_ADD; }
while { return TOKEN_WHILE; }
{LETTER}+ { return TOKEN_IDENT; }
{DIGIT}+ { return TOKEN_NUMBER; }

. { return TOKEN_ERROR; }
%%
int yywrap() { return 1; }

```
Figure 3.7: Example Flex Specification
```
**Contents of File: main.c**

#include "token.h"
#include <stdio.h>

extern FILE *yyin;
extern int yylex();
extern char *yytext;

int main() {
yyin = fopen("program.c","r");
if(!yyin) {
printf("could not open program.c!\n");
return 1;
}

while(1) {
token_t t = yylex();
if(t==TOKEN_EOF) break;
printf("token: %d text: %s\n",t,yytext);
}
}

```
Figure 3.8: Example Main Program
```

##### 30 CHAPTER 3. SCANNING

**Contents of File: token.h**

typedef enum {
TOKEN_EOF=0,
TOKEN_WHILE,
TOKEN_ADD,
TOKEN_IDENT,
TOKEN_NUMBER,
TOKEN_ERROR
} token_t;

```
Figure 3.9: Example Token Enumeration
```
```
Compiler scanner.o
```
```
Compiler main.o
```
```
Flex scanner.c
Linker scanner.exe
```
```
scanner.flex
```
```
main.c
```
```
token.h
```
```
Figure 3.10: Build Procedure for a Flex Program
```
be cleaned up after they are matched. For example,"hello\n"needs to
have its quotes removed and the backslash-n sequence converted to a lit-
eral newline character. Internally, the compiler only cares about the actual
contents of the string. Typically, this is accomplished by writing a function
stringcleanin the postamble of the Flex specification. The function is
invoked by the matching rule before returning the desired token type.
**Constraining tokens.** Although regular expressions can match tokens
of arbitrary length, it does not follow that a compiler must be prepared to
accept them. There would be little point to accepting a 1000-letter iden-
tifier, or an integer larger than the machine’s word size. The typical ap-
proach is to set the maximum token length (YYLMAXin flex) to a very large
value, then examine the token to see if it exceeds a logical limit in the ac-
tion that matches the token. This allows you to emit an error message that
describes the offending token as needed.
**Error Handling.** The easiest approach to handling errors or invalid
input is simply to print a message and exit the program. However, this
is unhelpful to users of your compiler – if there are multiple errors, it’s
(usually) better to see them all at once. A good approach is to match the


##### 3.9. EXERCISES 31

minimum amount of invalid text (using the dot rule) and return an explicit
token type indicating an error. The code that invokes the scanner can then
emit a suitable message, and then ask for the next token.

### 3.9 Exercises

1. Write regular expressions for the following entities. You may find it
    necessary to justify what is and is not allowed within each expres-
    sion:

```
(a) English days of the week: Monday, Tuesday, ...
(b) All integers where every three digits are separated by commas
for clarity, such as:
78
1,092
692,098,000
(c) Internet email addresses like:
"John Doe" <john.doe@gmail.com>
(d) HTTP Uniform Resource Locators (URLs)
as described by RFC-1738.
```
2. Write a regular expression for a string containing any number ofX
    and single pairs of< >and{ }which may be nested but not inter-
    leaved. For example these strings are allowed:
    XXX<XX{X}XXX>X
    X{X}X<X>X{X}X<X>X
    But these are not allowed:
    XXX<X<XX>>XX
    XX<XX{XX>XX}XX
3. Test the regular expressions you wrote in the previous two problems
    by translating them into your favorite programming language that
    has native support for regular expressions. (Perl and Python are two
    good choices.) Evaluate the correctness of your program by writing
    test cases that should (and should not) match.
4. Convert these REs into NFAs using Thompson’s construction:

```
(a)for | [a-z]+ | [xb]?[0-9]+
(b)a ( bc*d | ed ) d*
(c)( a*b | b*a | ba )*
```
5. Convert the NFAs in the previous problem into DFAs using the sub-
    set construction method.


##### 32 CHAPTER 3. SCANNING

6. Minimize the DFAs in the previous problem by using Hopcroft’s al-
    gorithm.
7. Write a hand-made scanner for JavaScript Object Notation (JSON)
    which is described athttp://json.org. The program should read
    JSON on the input, and then print out the sequence of tokens ob-
    served: LBRACKET, STRING, COLON, etc... Find some large JSON
    documents online and test your scanner to see if it works.
8. Using Flex, write a scanner for the Java programming language. As
    above, read in Java source on the input and output token types. Test
    it out by applying it to a large open source project written in Java.


##### 3.10. FURTHER READING 33

### 3.10 Further Reading

1. A.K. Dewdney, “The New Turing Omnibus: Sixty-Six Excursions in
    Computer Science”, Holt Paperbacks, 1992. An accessible overview of
    many fundamental problems in computer science – including finite state machines
    - collected from the author’s Mathematical Recreations column in Scientific Amer-
    ican.
2. S. Hollos and J.R. Hollos, “Finite Automata and Regular Expressions:
    Problems and Solutions”, Abrazol Publishing, 2013.
    A collection of clever little problems and solutions relating to automata and state
    machines, if you are looking for more problems to work on.
3. Marvin Minsky, “Computation: Finite and Infinite Machines”, Prentice-
    Hall, 1967.
    A classic text offering a more thorough introduction to the theory of finite automata
    at an undergraduate level.
4. S. Kleene, “Representation of events in nerve nets and finite automata”,
    Automata Studies, C. Shannon and J. McCarthy, editors, Princeton
    University Press, 1956.
5. R. McNaughton and H. Yamada, “Regular Expressions and State
    Graphs for Automata”, IRE Transactions on Electronic Computers,
    volume EC-9, number 1, 1960.
    [http://dx.doi.org/10.1109/TEC.1960.5221603](http://dx.doi.org/10.1109/TEC.1960.5221603)
6. K. Thompson, “Programming Techniques: Regular Expression Search
    Algorithm”, Communications of the ACM, volume 11, number 6,
    1968.
    [http://dx.doi.org/10.1145/363347.363387](http://dx.doi.org/10.1145/363347.363387)


##### 34 CHAPTER 3. SCANNING


##### 35

## Chapter 4 – Parsing

### 4.1 Overview

If scanning is like constructing words out of letters, then parsing is like
constructing sentences out of words in a natural language. Of course, not
every sequence of words makes a valid sentence: “horse aircraft conju-
gate” is three valid words, but not a meaningful sentence.
To parse a computer program, we must first describe the form of valid
sentences in a language. This formal statement is known as a context free
grammar (CFG). Because they allow for recursion, CFGs are more power-
ful than regular expressions and can express a richer set of structures.
While a plain CFG is relatively easy to write, it does not follow that
it is easy to parse. An arbitrary CFG can contain ambiguities and other
problems that make it difficult to write an automatic parser. Therefore, we
consider two subsets of CFGs known as LL(1) and LR(1) grammars.
LL(1) grammars are CFGs that can be evaluated by considering only
the current rule and next token in the input stream. This property makes
it easy to write a hand-coded parser known as a recursive descent parser.
However, a language (and its grammar) must be carefully designed (and
occasionally rewritten) in order to ensure that it is an LL(1) grammar. Not
all language structures can be expressed as LL(1) grammars.
LR(1) grammars are more general and more powerful than LL(1). Nearly
all useful programming languages can be written in LR(1) form. However,
the parsing algorithm for LR(1) grammars is more complex and usually
cannot be written by hand. Instead, it is common to use a parser generator
that will accept an LR(1) grammar and automatically generate the parsing
code.


##### 36 CHAPTER 4. PARSING

### 4.2 Context Free Grammars

Let’s begin by defining the parts of a CFG.
A **terminal** is a discrete symbol that can appear in the language, other-
wise known as a token from the previous chapter. Examples of terminals
are keywords, operators, and identifiers. We will use lower-case letters to
represent terminals. At this stage, we only need to consider the kind (e.g.
integer literal) and not the value (e.g. 456 ) of a terminal.
A **non-terminal** represents a structure that can occur in a language,
but is not a literal symbol. Example of non-terminals are declarations,
statements, and expressions. We will use upper-case letters to represent
non-terminals:Pfor program,Sfor statement,Efor expression, etc.
A **sentence** is a valid sequence of terminals in a language, while a **sen-
tential form** is a valid sequence of terminals and non-terminals. We will
use Greek symbols to represent sentential forms. For example,α,β, andγ
represent (possibly) mixed sequences of terminals and non-terminals. We
will use a sequence likeY 1 Y 2 ...Ynto indicate the individual symbols in a
sentential form:Yimay be either a terminal or a non-terminal.
A **context-free grammar (CFG)** is a list of **rules** that formally describe
the allowable sentences in a language. The left-hand side of each rule is
always a single non-terminal. The right-hand side of a rule is a sentential
form that describes an allowable form of that non-terminal. For example,
the rule A→xXy indicates that the non-terminalArepresents a terminal
xfollowed by a non-terminalXand a terminaly. The right hand side of
a rule can beto indicate that the rule produces nothing. The first rule is
special: it is the top-level definition of a program and its non-terminal is
known as the **start symbol**.
For example, here is a simple CFG that describes expressions involving
addition, integers, and identifiers:

```
Grammar G 2
```
1. P→E
2. E→E + E
3. E→ident
4. E→int

This grammar can be read as follows: (1) A complete program consists
of one expression. (2) An expression can be any expression plus any ex-
pression. (3) An expression can be an identifier. (4) An expression can be
an integer literal.
For brevity, we occasionally condense a set of rules with a common
left-hand side by combining all of the right hand sides with a logical-or
symbol, like this:
E→E+E|ident|int


##### 4.2. CONTEXT FREE GRAMMARS 37

**4.2.1 Deriving Sentences**

Each grammar describes a (possibly infinite) set of sentences, which is
known as the **language** of the grammar. To prove that a given sentence is
a member of that language, we must show that there exists a sequence of
rule applications that connects the start symbol with the desired sentence.
A sequence of rule applications is known as a **derivation** and a double ar-
row (⇒) is used to show that one sentential form is equal to another by
applying a given rule. For example:

- E⇒intby applying rule 4 of GrammarG 2.
- E + E⇒E + identby applying rule 3 of GrammarG 2.
- P⇒int + ident by applying all rules of GrammarG 2.

There are two approaches to derivation: top-down and bottom-up.
In **top-down derivation** , we begin with the start symbol, and then ap-
ply rules in the CFG to expand non-terminals until reaching the desired
sentence. For example, **ident + int + int** is a sentence in this language, and
here is one derivation to prove it:

```
Sentential Form Apply Rule
P P→E
E E→E + E
E + E E→ident
ident + E E→E + E
ident + E + E E→int
ident + int + E E→int
ident + int + int
```
In **bottom-up derivation** , we begin with the desired sentence, and then
apply the rules backwards until reaching the start symbol. Here is a bottom-
up derivation of the same sentence:

```
Sentential Form Apply Rule
ident + int + int E→int
ident + int + E E→int
ident + E + E E→E+E
ident + E E→ident
E + E E→E+E
E P→E
P
```
Be careful to distinguish between agrammar(which is a finite set of
rules) and alanguage(which is a set of strings generated by a grammar).
It is quite possible for two different grammars to generate the same lan-
guage, in which case we describe them as having **weak equivalence**.


##### 38 CHAPTER 4. PARSING

#### 4.2.2 Ambiguous Grammars

An **ambiguous grammar** allows for more than one possible derivation of
the same sentence. Our example grammar is ambiguous because there are
two possible derivations for any sentence involving two plus signs. The
sentenceident + int + intcan have the two derivations shown in
Figure 4.1.

```
Left-Most Derivation Right-Most Derivation
```
```
P
```
```
E
```
```
E + E
```
```
E + E int
```
```
ident int
```
```
P
```
```
E
```
```
E + E
```
```
ident E + E
```
```
int int
```
```
Figure 4.1: Two Derivations of the Same Sentence
```
Ambiguous grammars present a real problem for parsing (and lan-
guage design in general) because we do not want a program to have two
possible meanings.
Does it matter in this example? It certainly does! In a language like
Java, the + operator indicates not only addition between integers, but also
concatenation between strings. If the identifier ishelloand the two in-
tegers have the value 5 , then the left-most derivation would concatenate
all three together intohello55, while the right-most derivation would
compute5+5=10and concatenate the result intohello10.


##### 4.2. CONTEXT FREE GRAMMARS 39

Fortunately, it is usually possible to re-write a grammar so that it is not
ambiguous. In the common case of binary operators, we can require that
one side of the expression be an atomic term (T), like this:

```
Grammar G 3
```
1. P→E
2. E→E + T
3. E→T
4. T→ident
5. T→int

With this change, the grammar is no longer ambiguous, because it only
allows a left-most derivation. But also note that itstill accepts the same lan-
guage as GrammarG 2. That is, any sentence that can be derived by Gram-
marG 2 can also be derived by GrammarG 3 , but there exists only one
derivation (and one meaning) per sentence. (Proof is left as an exercise to
the reader.)
Now suppose that we would like to add more operators to our gram-
mar. If we simply add more rules of the form E→E∗T and E→E÷T, we
would still have an unambiguous grammar, but it would not follow the
rules of precedence in algebra: each operator would be applied from left
to right.
Instead, the usual approach is to construct a grammar with multiple
levels that reflect the intended precedence of operators. For example, we
can combine addition and multiplication by expressing them as a sum of
terms (T) that consist of multiplied factors (F), like this:

```
Grammar G 4
```
1. P→E
2. E→E + T
3. E→T
4. T→T * F
5. T→F
6. F→ident
7. F→int

Here is another common example that occurs in most programming
languages in some form or another. Suppose that anifstatement has two
variations: an if-then which takes an action when an expression is true,
and an if-then-else that takes a different action for the true and false cases.
We can express this fragment of the language like this:


##### 40 CHAPTER 4. PARSING

```
Grammar G 5
```
1. P→S
2. S→if E then S
3. S→if E then S else S
4. S→other

GrammarG 5 is ambiguous because it allows for two derivations of
this sentence: if E then if E then other else other. Do you
see the problem? Theelsepart could belong to the outerifor to the in-
nerif. In most programming languages, theelseis defined as belonging
to the innerif, but the grammar does not reflect this.

```
Do this now:
Write out the two possible parse trees for this sentence:
if E then if E then other else other.
```
### 4.3 LL Grammars

LL(1) grammars are a subset of CFGs that are easy to parse with simple
algorithms. A grammar is LL(1) if it can be parsed by considering only
one non-terminal and the next token in the input stream.
To ensure that a grammar is LL(1), we must do the following:

- Remove any ambiguity, as shown above.
- Eliminate any left recursion, as shown below.
- Eliminate any common left prefixes, as shown below.

Once we have taken those steps, then we can prove that it is LL(1) by
generating the FIRSTand FOLLOWsets for the grammar, and using them
to create the LL(1) parse table. If the parse table contains no conflicts, then
the grammar is clearly LL(1).


##### 4.3. LL GRAMMARS 41

#### 4.3.1 Eliminating Left Recursion

LL(1) grammars cannot contain **left recursion** , which is a rule of the form
A→Aαor, more generally, any ruleA→Bβsuch thatB⇒Aγ by some
sequence of derivations. For example, the rule E→E + T is left-recursive
becauseEappears as the first symbol on the right hand side.
You might be tempted to solve the problem by simply re-writing the
rule as E→T + E. While that would avoid left recursion, it would not
be an equivalent grammar because it would result in a right-associative
plus operator. Also, it would introduce the new problem of a common left
prefix, discussed below.
Informally, we must re-write the rules so that the (formerly) recursive
rule begins with the leading symbols of its alternatives.
Formally, if you have a grammar of the form:

```
A→Aα 1 |Aα 2 |...|β 1 |β 2 |...
```
```
Substitute with:
```
```
A→β 1 A′|β 2 A′|...
```
```
A’→α 1 A′|α 2 A′|...|
```
```
Applying this rule to grammar GrammarG 3 , we can re-write it as:
```
```
Grammar G 6
```
1. P→E
2. E→T E’
3. E’→+ T E’
4. E’→
5. T→ident
6. T→int

While GrammarG 6 is perhaps slightly harder for a person to read, it
no longer contains left recursion, and satisfies all the LL(1) properties. A
parser considering anEin rule 2 will immediately consider theTnon-
terminal, and then look atidentorinton the input to decide between
rule 5 and 6. After consideringT, the parser moves on to considerE′and
can distinguish between rule 3 and 4 by looking for either a+or any other
symbol on the input.


##### 42 CHAPTER 4. PARSING

#### 4.3.2 Eliminating Common Left Prefixes

A simpler problem to solve is grammars that have multiple rules with
the same left hand side and a common prefix of tokens on the right hand
side. Informally, we simply look for all common prefixes of a given non-
terminal, and replace them with one rule that contains the prefix and an-
other that contains the variants.
Formally, look for rules of this form:

```
A→αβ 1 |αβ 2 |...
```
```
And replace with:
```
```
A→αA′
```
```
A′→β 1 |β 2 |...
```
For example, these rules describing an identifier, array reference, and
function call all share the same prefix of a single identifier:

```
Grammar G 7
```
1. P→E
2. E→id
3. E→id [ E ]
4. E→id ( E )

If a parser is evaluatingEand sees anidon the input, that information
is not sufficient to distinguish between rules 2, 3, and 4. However, the
grammar can be salvaged by factoring out the common prefix, like this:

```
Grammar G 8
```
1. P →E
2. E →id E’
3. E’→[ E ]
4. E’→( E )
5. E’→

In this formulation, the parser always consumes anidwhen evaluat-
ing anE. If the next token is[, then rule 3 is applied. If the next token is
(, then rule 4 is applied; otherwise, rule 5 is applied.


##### 4.3. LL GRAMMARS 43

#### 4.3.3 First and Follow Sets

In order to construct a complete parser for an LL(1) grammar, we must
compute two sets, known as FIRSTand FOLLOW. Informally, FIRST(α)
indicates the set of terminals (including) that could potentially appear
at the beginning of any derivation ofα. FOLLOW(A) indicates the set of
terminals (including $) that could potentially occur after any derivation of
the non-terminalA. Given the contents of these sets, an LL(1) parser will
always know which rule to pick next.
Here is how to compute FIRSTand FOLLOW:

```
Computing First Sets for a Grammar G
```
```
FIRST(α) is the set of terminals that begin all strings given byα,
includingifα⇒.
```
```
For Terminals:
For each terminala∈Σ: FIRST(a)={a}
```
```
For Non-Terminals:
Repeat:
For each ruleX→Y 1 Y 2 ...Ykin a grammarG:
Addato FIRST(X)
ifais in FIRST(Y 1 )
orais in FIRST(Yn) andY 1 ...Yn− 1 ⇒
IfY 1 ...Yk⇒then addto FIRST(X).
until no more changes occur.
```
```
For a Sentential Form α :
For each symbolY 1 Y 2 ...Ykinα:
Addato FIRST(α)
ifais in FIRST(Y 1 )
orais in FIRST(Yn) andY 1 ...Yn− 1 ⇒
IfY 1 ...Yk⇒then addto FIRST(α).
```

##### 44 CHAPTER 4. PARSING

```
Computing Follow Sets for a Grammar G
```
```
FOLLOW(A) is the set of terminals that can come after
non-terminal A, including $ ifAoccurs at the end of the input.
```
```
FOLLOW(S)={$}whereSis the start symbol.
```
```
Repeat:
If A→αBβthen:
add FIRST(β) (excepting) to FOLLOW(B).
If A→αBor FIRST(β) containsthen:
add FOLLOW(A) to FOLLOW(B).
until no more changes occur.
```
```
Here is an example of computing FIRSTand FOLLOWfor GrammarG 9 :
```
```
Grammar G 9
```
1. P→E
2. E→T E’
3. E’→+ T E’
4. E’→
5. T→F T’
6. T’→* F T’
7. T’→
8. F→( E )
9. F→int

```
First and Follow for Grammar G 9
```
```
P E E’ T T’ F
FIRST ( int ( int + ( int * ( int
FOLLOW $ ) $ ) $ + ) $ + ) $ + * ) $
```
Once we have cleaned up a grammar to be LL(1) and computed its
FIRSTand FOLLOWsets, we are ready to write code for a parser. This can
be done by hand or with a table-driven approach.


##### 4.3. LL GRAMMARS 45

#### 4.3.4 Recursive Descent Parsing

LL(1) grammars are very amenable to writing simple hand-coded parsers.
A common approach is a **recursive descent parser** in which there is one
simple function for each non-terminal in the grammar. The body of the
function follows the right-hand sides of the corresponding rules: non-
terminals result in a call to another parse function, while terminals result
in considering the next token.
Three helper functions are needed:

- scantoken()returns the next token on the input stream.
- putbacktoken(t)puts an unexpected token back on the input
    stream, where it will be read again by the next call toscantoken.
- expecttoken(t)callsscantokento retrieve the next token. It
    returns true if the token matches the expected type. If not, it puts the
    token back on the input stream and returns false.

Figure 4.2 shows how GrammarG 9 could be written as a recursive de-
scent parser. Note that the parser has one function for each non-terminal:
parseP,parseE, etc. Each function returns true (1) if the input matches
the grammar, or false (0) otherwise.
Two special cases should be considered. First, if a ruleXcannot pro-
duceand we encounter a token not in FIRST(X), then we have definitely
encountered a parsing error, and we should display a message and return
failure. Second, if a ruleXcouldproduceand we encounter a token not
in FIRST(X), then we accept the rule X→put the token back on the input,
and return success. Another rule will expect to consume that token.
There is also the question of what the parser should actuallydoafter
matching some element of the grammar. In our simple example, the parser
simply returns true on a match, and serves only to verify that the input
program matches the grammar. If we wished to actually evaluate the ex-
pression, eachparseXfunction could compute the result and return it
as adouble. This would effectively give us a simple interpreter for this
language. Another approach is for eachparseXfunction to return a data
structure representing that node of the parse tree. As each node is parsed,
the result is assembled into an abstract syntax tree, with the root returned
byparseP.


##### 46 CHAPTER 4. PARSING

int parse_P() {
return parse_E() && expect_token(TOKEN_EOF);
}

int parse_E() {
return parse_T() && parse_E_prime();
}

int parse_E_prime() {
token_t t = scan_token();
if(t==TOKEN_PLUS) {
return parse_T() && parse_E_prime();
} else {
putback_token(t);
return 1;
}
}

int parse_T() {
return parse_F() && parse_T_prime();
}

int parse_T_prime() {
token_t t = scan_token();
if(t==TOKEN_MULTIPLY) {
return parse_F() && parse_T_prime();
} else {
putback_token(t);
return 1;
}
}

int parse_F() {
token_t t = scan_token();
if(t==TOKEN_LPAREN) {
return parse_E() && expect_token(TOKEN_RPAREN);
} else if(t==TOKEN_INT) {
return 1;
} else {
printf("parse error: unexpected token %s\n",
token_string(t));
return 0;
}
}

```
Figure 4.2: A Recursive-Descent Parser
```

##### 4.3. LL GRAMMARS 47

#### 4.3.5 Table Driven Parsing

An LL(1) grammar can also be parsed using generalized table driven code.
A table-driven parser requires a grammar, a parse table, and a stack to
represent the current set of non-terminals.
The **LL(1) parse table** is used to determine which rule should be ap-
plied for any combination of non-terminal on the stack and next token on
the input stream. (By definition, an LL(1) grammar has exactly one rule to
be applied for each combination.) To create a parse table, we use the FIRST
and FOLLOWsets like this:

```
LL(1) Parse Table Construction.
Given a grammarGand alphabetΣ, create a parse tableT[A,a]
that selects a rule for each combination of non-terminalA∈G
and terminala∈Σ.
```
```
For each ruleA→αinG:
For each terminala(excepting) in FIRST(α):
AddA→αtoT[A,a].
Ifis in FIRST(α):
For each terminalb(including $) in FOLLOW(A):
Add A→αtoT[A,b].
```
For example, here is the parse table for GrammarG 9. Notice that the
entries forP,E,T, andFare straightforward: each can only start with
intor(, and so these tokens cause the rules to descend towardFand a
choice between rule 8 (F→int) and rule 9 (F→(E)). The entry forE′is
a little more complicated: a+token results in applyingE′→+TE′, while
)or$indicatesE′→.

```
Parse Table for Grammar G 9 :
```
```
int + * ( ) $
P 1 1
E 2 2
E’ 3 4 4
T 5 5
T’ 7 6 7 7
F 9 8
```

##### 48 CHAPTER 4. PARSING

Now we have all the pieces necessary to operate the parser. Informally,
the idea is to keep a stack that tracks the current state of the parser. In each
step, we consider the top element of the stack and the next token on the
input. If they match, then pop the stack, accept the token and continue.
If not, then consult the parse table for the next rule to apply. If we can
continue until the end-of-file symbol is matched, then the parse succeeds.

```
LL(1) Table Parsing Algorithm.
Given a grammarGwith start symbolPand parse tableT,
parse a sequence of tokens and determine whether they satisfyG.
```
```
Create a stackS.
Push $ andPontoS.
Letcbe the first token on the input.
```
```
WhileSis not empty:
LetXbe the top element of the stack.
IfXmatchesc:
RemoveXfrom the stack.
Advancecto the next token and repeat.
IfXis any other terminal, stop with an error.
IfT[X,c]indicates ruleX→α:
RemoveXfrom the stack.
Push symbolsαon to the stack and repeat.
IfT[X,c]indicates an error state, stop with an error.
```
Here is an example of the algorithm applied to the sentenceint * int:

```
Stack Input Action
P $ int * int $ apply 1:P⇒E
E $ int * int $ apply 2:E⇒T E’
T E’ $ int * int $ apply 5:T⇒F T’
F T’ E’ $ int * int $ apply 9:F⇒int
int T’ E’ $ int * int $ matchint
T’ E’ $ * int $ apply 6:T’⇒* F T’
* F T’ E’ $ * int $ match*
F T’ E’ $ int $ apply 9:F⇒int
int T’ E’ $ int $ matchint
T’ E’ $ $ apply 7:T’⇒
E’ $ $ apply 4:E’⇒
$ $ match$
```

##### 4.4. LR GRAMMARS 49

### 4.4 LR Grammars

While LL(1) grammars and top-down parsing techniques are easy to work
with, they are not able to represent all of the structures found in many
programming languages. For more general-purpose programming lan-
guages, we must use an LR(1) grammar and associated bottom-up parsing
techniques.
LR(1) is the set of grammars that can be parsed via shift-reduce tech-
niques with a single character of lookahead. LR(1) is a super-set of LL(1)
and can accommodate left recursion and common left prefixes which are
not permitted in LL(1). This enables us to express many programming
constructs in a more natural way. (An LR(1) grammar must still be non-
ambiguous, and it cannot have shift-reduce or reduce-reduce conflicts,
which we will explain below.)
For example, GrammarG 10 is an LR(1) grammar:

```
Grammar G 10
```
1. P→E
2. E→E + T
3. E→T
4. T→id ( E )
5. T→id

We need to know the FIRSTand FOLLOWsets of LR(1) grammars as
well, so take a moment now and work out the sets for GrammarG 10 , using
the same technique from section 4.3.3.

##### P E T

##### FIRST

##### FOLLOW


##### 50 CHAPTER 4. PARSING

#### 4.4.1 Shift-Reduce Parsing

LR(1) grammars must be parsed using the **shift-reduce** parsing technique.
This is a bottom-up parsing strategy that begins with the tokens and looks
for rules that can be applied to reduce sentential forms into non-terminals.
If there is a sequence of reductions that leads to the start symbol, then the
parse is successful.
A **shift** action consumes one token from the input stream and pushes
it onto the stack. A **reduce** action applies one rule of the form A→α
from the grammar, replacing the sentential formαon the stack with the
non-terminalA. For example, here is a shift-reduce parse of the sentence
id(id+id)using GrammarG 10 :

```
Stack Input Action
id ( id + id) $ shift
id ( id + id ) $ shift
id ( id + id ) $ shift
id ( id + id ) $ reduce T→id
id ( T + id ) $ reduce E→T
id ( E + id ) $ shift
id ( E + id ) $ shift
id ( E + id ) $ reduce T→id
id ( E + T ) $ reduce E→E + T
id ( E ) $ shift
id ( E ) $ reduce T→id(E)
T $ reduce E→T
E $ reduce P→E
P $ accept
```
While this example shows that there exists a derivation for the sen-
tence, it does not explain how each action was chosen at each step. For
example, in the second step, we might have chosen to reduceidtoTin-
stead of shifting a left parenthesis. This would have been a bad choice,
because there is no rule that begins withT(, but that was not immediately
obvious without attempting to proceed further. To make these decisions,
we must analyze LR(1) grammars in more detail.


##### 4.4. LR GRAMMARS 51

#### 4.4.2 The LR(0) Automaton

An **LR(0) automaton** represents all the possible rules that are currently
under consideration by a shift-reduce parser. (The LR(0) automaton is also
variously known as the **canonical collection** or the **compact finite state
machine** of the grammar.) Figure 4.6 shows a complete automaton for
GrammarG 10. Each box represents a state in the machine, connected by
transitions for both terminals and non-terminals in the grammar.
Each state in the automaton consists of multiple **items** , which are rules
augmented by a **dot** (.) that indicates the parser’s current position in that
rule. For example, the configuration E→E. + T indicates thatEis cur-
rently on the stack, and+ Tis a possible next sequence of tokens.
The automaton is constructed as follows. State 0 is created by taking
the production for the start symbol (P→E) and adding a dot at the begin-
ning of the right hand side. This indicates that we expect to see a complete
program, but have not yet consumed any symbols. This is known as the
**kernel** of the state.

```
Kernel of State 0
```
##### P→. E

Then, we compute the **closure** of the state as follows. For each item
in the state with a non-terminalXimmediately to the right of the dot, we
add all rules in the grammar that haveXas the left hand side. The newly
added items have a dot at the beginning of the right hand side.

##### P→. E

##### E→. E + T

##### E→. T

```
The procedure continues until no new items can be added:
```
```
Closure of State 0
```
```
P→. E
E→. E + T
E→. T
T→. id ( E )
T→. id
```

##### 52 CHAPTER 4. PARSING

You can think of the state this way: It describes the initial state of the
parser as expecting a complete program in the form of anE. However, an
Eis known to begin with anEor aT, and aTmust begin with anid. All
of those symbols could represent the beginning of the program.
From this state, all of the symbols (terminals and non-terminals both)
to the right of the dot are possible outgoing transitions. If the automa-
ton takes that transition, it moves to a new state containing the matching
items, with the dot moved one position to the right. The closure of the new
state is computed, possibly adding new rules as described above.
For example, from state zero,E,T, andidare the possible transitions,
because each appears to the right of the dot in some rule. Here are the
states for each of those transitions:

```
Transition on E:
```
```
P→E.
E→E. + T
```
```
Transition on T:
```
```
E→T.
```
```
Transition on id:
```
```
T→id. ( E )
T→id.
```
Figure 4.3 gives the complete LR(0) automaton for GrammarG 10. Take
a moment now to trace over the table and be sure that you understand
how it is constructed.

```
No, really. Stop now and study the figure carefully before continuing.
```

##### 4.4. LR GRAMMARS 53

```
start accept
```
```
State 0
P →. E
E →. E + T
E →. T
T →. id ( E )
T →. id
```
```
State 1
P → E.
E → E. + T
```
```
E
```
```
State 8
E → T.
```
```
T
```
```
State 4
T → id. ( E )
T → id.
```
```
id
```
```
State 2
E → E +. T
T →. id ( E )
T →. id
```
```
+
```
```
$
```
```
State 3
E → E + T.
```
```
id T
```
```
State 5
T → id (. E )
E →. E + T
E →. T
T →. id (E)
T →. id
```
```
(
```
```
State 6
T → id ( E. )
E → E. + T
```
```
T E
```
```
id
```
```
State 7
T → id ( E ).
```
```
)
```
```
+
```
```
Figure 4.3: LR(0) Automaton for Grammar G 10
```

##### 54 CHAPTER 4. PARSING

The LR(0) automaton tells us the choices available at any step of bot-
tom up parsing. When we reach a state containing an item with a dot at
the end of the rule, that indicates a possible reduction. A transition on a
terminal that moves the dot one position to the right indicates a possible
shift. While the LR(0) automaton tells us the available actions at each step,
it does not always tell uswhichaction to take.^1
Two types of conflicts can appear in an LR grammar:
A **shift-reduce conflict** indicates a choice between a shift action and a
reduce action. For example, state 4 offers a choice between shifting a left
parenthesis and reducing by rule five:

```
Shift-Reduce Conflict:
T→id. ( E )
T→id.
```
A **reduce-reduce conflict** indicates that two distinct rules have been
completely matched, and either one could apply. While GrammarG 10
does not contain any reduce-reduce conflicts, they commonly occur when
a syntactic structure occurs at multiple layers in a grammar. For example,
it is often the case that a function invocation can be a statement by itself
or an element within an expression. The automaton for such a grammar
would contain a state like this:

```
Reduce-Reduce Conflict:
S→id ( E ).
E→id ( E ).
```
The LR(0) automaton forms the basis of LR parsing, by telling us which
actions are available in each state. But, it does not tell uswhichaction to
take or how to resolve shift-reduce and reduce-reduce conflicts. To do that,
we must take into account some additional information.

(^1) The 0 in LR(0) indicates that it uses zero lookahead tokens, which is a way of saying that
it does not consider the next token before making a reduction. While it is possible to write
out a grammar that is strictly LR(0), such a grammar has very limited utility.


##### 4.4. LR GRAMMARS 55

#### 4.4.3 SLR Parsing

**Simple LR (SLR)** parsing is a basic form of LR parsing in which we use
FOLLOWsets to resolve conflicts in the LR(0) automaton. In short, we
take the reduction A→αonly when the next token on the input is in
FOLLOW(A). If a grammar can be parsed by this technique, we say it is an
**SLR grammar** , which is a subset of LR(1) grammars.
For example, the shift-reduce conflict in state 4 of Figure 4.6 is resolved
by consulting FOLLOW(T). If the next token is+,)or $, then we reduce
by rule T→id. If the next token is(, then we shift to state 5. If neither of
those is true, then the input is invalid, and we emit a parse error.
These decisions are encoded in the **SLR parse tables** which are known
historically as GOTOand ACTION. The tables are created as follows:

```
SLR Parse Table Creation.
```
```
Given a grammarGand corresponding LR(0) automaton,
create tables ACTION[s,a] and GOTO[s,A] for all statess,
terminalsa, and non-terminalsAinG.
```
```
For each state s:
For each item like A→α. aβ
ACTION[s,a] = shift to statetaccording to the LR(0) automaton.
For each item like A→α. Bβ
GOTO[s,B] = goto statetaccording to the LR(0) automaton.
For each item like A→α.
For each terminalain FOLLOW(A):
ACTION[s,a] = reduce by rule A→α
```
```
All remaining states are considered error states.
```
Naturally, each state in the table can be occupied by only one action.
If following the procedure results in a table with more than one state in a
given entry, then you can conclude that the grammar is not SLR. (It might
still be LR(1) – more on that below.)
Here is the SLR parse table for GrammarG 10. Note carefully the states
1 and 4 where there is a choice between shifting and reducing. In state 1, a
lookahead of+causes a shift, while a lookahead of $ results in a reduction
P→E because $ is the only member of FOLLOW(P).


##### 56 CHAPTER 4. PARSING

```
State GOTO ACTION
E T id ( ) + $
0 G1 G8 S4
1 S2 R1
2 G3 S4
3 R2 R2 R2
4 S5 R5 R5 R5
5 G6 G8 S4
6 S7 S2
7 R4 R4 R4
8 R3 R3 R3
```
```
Figure 4.4: SLR Parse Table for Grammar G 10
```
Now we are ready to parse an input by following the SLR parsing al-
gorithm. The parse requires maintaining a stack of states in the LR(0) au-
tomaton, initially containing the start stateS 0. Then, we examine the top
of the stack and the lookahead token, and take the action indicated by the
SLR parse table. On a shift, we consume the token and push the indi-
cated state on the stack. On a reduce by A→β, we pop states from stack
corresponding to each of the symbols inβ, then take the additional step of
moving to state GOTO[t,A]. This process continues until we either succeed
by reducing to the start symbol, or fail by encountering an error state.

```
SLR Parsing Algorithm.
```
```
LetSbe a stack of LR(0) automaton states. PushS 0 ontoS.
Letabe the first input token.
```
```
Loop:
Letsbe the top of the stack.
If ACTION[s,a] is accept :
Parse complete.
Else if ACTION[s,a] is shift t:
Push stateton the stack.
Letabe the next input token.
Else if ACTION[s,a] is reduce A→β:
Pop states corresponding toβfrom the stack.
Lettbe the top of stack.
Push GOTO[t,A] onto the stack.
Otherwise:
Halt with a parse error.
```

##### 4.4. LR GRAMMARS 57

Here is an example of applying the SLR parsing algorithm to the pro-
gramid ( id + id ). The first three steps are easy: a shift is per-
formed for each of the first three tokensid ( id. The fourth step re-
duces T→id. This causes state 4 (corresponding to the right hand side
id) to be popped from the stack. State 5 is now at the top of the stack, and
GOTO[ 5 ,T]= 8, so state 8 is pushed, resulting in a stack of0 4 5 8.

```
Stack Symbols Input Action
0 id ( id + id) $ shift 4
0 4 id ( id + id ) $ shift 5
0 4 5 id ( id + id ) $ shift 4
0 4 5 4 id ( id + id ) $ reduce T→id
0 4 5 8 id ( T + id ) $ reduce E→T
0 4 5 6 id ( E + id ) $ shift 2
0 4 5 6 2 id ( E + id ) $ shift 4
0 4 5 6 2 4 id ( E + id ) $ reduce T→id
0 4 5 6 2 3 id ( E + T ) $ reduce E→E + T
0 4 5 6 id ( E ) $ shift 7
0 4 5 6 7 id ( E ) $ reduce T→id(E)
0 8 T $ reduce E→T
0 1 E $ accept
```
(Although we show two columns for “Stack” and “Symbols”, they
are simply two representations of the same information. The stack state
0 4 5 8represents the parse state ofid ( Tand vice versa.)
It should now be clear that SLR parsing has the same algorithmic com-
plexity as LL(1) parsing. Both techniques require a parsing table and a
stack. At each step in both algorithms, it is necessary to only consider the
current state and the next token on the input. The distinction is that each
LL(1) parsing state considers only a single non-terminal, while each LR(1)
parsing state considers a large number of possible configurations.


##### 58 CHAPTER 4. PARSING

SLR parsing is a good starting point for understanding the general
principles of bottom up parsing. However, SLR is a subset of LR(1), and
not all LR(1) grammars are SLR. For example, consider GrammarG 11
which allows for a statement to be a variable assignment, or an identifier
by itself. Note that FOLLOW(S)={$}and FOLLOW(V)={=]$}.

```
Grammar G 11
```
1. S→V = E
2. S→id
3. V→id
4. V→id [ E ]
5. E→V

```
We need only build part of the LR(0) automaton to see the problem:
```
```
State 0
S ->. V = E
S ->. id
V ->. id
V ->. id [ E ]
```
```
State 1
S -> id.
V -> id.
id V -> id. [ E ]
```
##### ...

##### V

##### [ ...

```
Figure 4.5: Part of LR(0) Automaton for Grammar G 11
```
In state 1, we can reduce by S→id or V→id. However, both FOLLOW(S)
and FOLLOW(V) contain $, so we cannot decide which to take when the
next token is end-of-file. Even using the FOLLOWsets, there is still a
reduce-reduce conflict. Therefore, GrammarG 11 is not an SLR grammar.
But, if we look more closely at the possible sentences allowed by the
grammar, the distinction between the two becomes clear. Rule S→id
would only be applied in the case where the complete sentence isid $.
If any other character follows a leadingid, then V→id applies. So, the
grammar is not inherently ambiguous: we just need a more powerful pars-
ing algorithm.


##### 4.4. LR GRAMMARS 59

#### 4.4.4 LR(1) Parsing

The LR(0) automaton is limited in power, because it does not track what
tokens can actually follow a production. SLR parsing accommodates for
this weakness by using FOLLOWsets to decide when to reduce. As shown
above, this is not sufficiently discriminating to parse all LR(1) grammars.
Now we give the complete or “canonical” form of LR(1) parsing, which
depends upon the LR(1) automaton. The LR(1) automaton is like the LR(0)
automaton, except that each item is annotated with the set of tokens that
could potentially follow it, given the current parsing state. This set is
known as the **lookahead** of the item. The lookahead is always a subset
of the FOLLOWof the relevant non-terminal.
The lookahead of the kernel of the start state is always{$}. When com-
puting the closure of a state, we consider two cases:

- For an item like A→α.Bwith a lookahead of{L}, add new rules
    like B→.γwith a lookahead of{L}.
- For an item like A→α.Bβ, with a lookahead of{L}, add new rules
    like B→.γwith a lookahead as follows:
       **-** Ifβ **cannot** produce, the lookahead is FIRST(β).
       **-** Ifβ **can** produce, the lookahead is FIRST(β)∪{L}.

As before, the rules like B→.γto be added to the state correspond to
all of the rules in the grammar with B on the left hand side.

Here is an example for GrammarG 11. The kernel of the start state
consists of the start symbol with a lookahead of $:

```
Kernel of State 0
```
```
S→. V = E{$}
S→. id {$}
```
The closure of the start state is computed by adding the rules forV
with a lookahead of=, because=followsVin rule 1:

```
Closure of State 0
```
```
S→. V = E {$}
S→. id {$}
V→. id {=}
V→. id [ E ]{=}
```
Now suppose that we construct state 1 via a transition on the terminal
id. The lookahead for each item is propagated to the new state:


##### 60 CHAPTER 4. PARSING

```
Closure of State 1
```
```
S→id. {$}
V→id. {=}
V→id. [ E ]{=}
```
Now you can see how the lookahead solves the reduce-reduce conflict.
When the next token on the input is $, we can only reduce by S→id.
When the next token is=, we can only reduce by V→id. By tracking
lookaheads in a more fine-grained manner than SLR, we are able to parse
arbitrary LR(1) grammars.
Figure 4.6 gives the complete LR(1) automaton for GrammarG 10. Take
a moment now to trace over the table and be sure that you understand
how it is constructed.
One aspect of state zero is worth clarifying. When constructing the
closure of a state, we must considerallrules in the grammar, including
the rule corresponding to the item under closure. The item E→. E + T
is initially added with a lookahead of{$}. Then, evaluating that item, we
add all rules that haveEon the left hand side, adding a lookahead of{+}.
So, we add E→. E + Tagain, this time with a lookahead of{+}, resulting
in a single item with a lookahead set of{$,+}

```
Once again: Stop now and study the figure carefully before continuing.
```

##### 4.4. LR GRAMMARS 61

```
start accept
```
```
State 0
P →. E {$}
E →. E + T {$,+}
E →. T {$,+}
T →. id ( E ) {$,+}
T →. id {$,+}
```
```
State 1
P → E. {$}
E → E. + T {$,+}
```
```
E
```
```
State 4
T → id. ( E ) {$,+}
T → id. {$,+}
```
```
id
```
```
State 8
E → T. {$,+}
```
```
T
```
```
State 2
E → E +. T {$,+}
T →. id ( E ) {$,+}
T →. id {$,+}
```
```
+
```
```
$
```
```
State 5
T → id (. E ) {$,+}
E →. E + T {+,) }
E →. T {+,) }
T →. id (E) {+,) }
T →. id {+,) }
```
```
(
```
```
State 3
E → E + T. {$,+}
```
```
id T
```
```
State 6
T → id ( E. ) {$,+}
E → E. + T {+,) }
```
```
E
```
```
State 15
E → T. {+,) }
```
```
T T → id. ( E ) {+,) }State 9
T → id. {+,) }
```
```
id
```
```
State 7
T → id ( E ). {$,+}
```
```
)
```
```
State 10
E → E +. T {+,) }
T →. id ( E ) {+,) }
T →. id {+,) }
```
```
+
```
```
State 12
T → id (. E ) {+,) }
E →. E + T {+,) }
E →. T {+,) }
T →. id (E) {+,) }
T →. id {+,) }
```
```
( id
```
```
State 11
E → E + T. {+,) }
```
```
T
```
```
id
```
```
State 13
T → id ( E. ) {+,) }
E → E. + T {+,) }
```
```
E
```
```
T
```
```
+
```
```
State 14
T → id ( E ). {+,) }
```
```
)
```
```
Figure 4.6: LR(1) Automaton for Grammar G 10
```

##### 62 CHAPTER 4. PARSING

#### 4.4.5 LALR Parsing

The main downside to LR(1) parsing is that the LR(1) automaton can be
muchlarger than the LR(0) automaton. Any two states that have the same
items but differ in lookahead sets foranyitems are considered to be differ-
ent states. The result is enormous parse tables that consume large amounts
of memory and slow down the parsing algorithm.
**Lookahead LR (LALR)** parsing is the practical answer to this problem.
To construct an LALR parser, we first create the LR(1) automaton, and then
merge states that have the same core. The **core** of a state is simply the body
of an item, ignoring the lookahead. When several LR(1) items are merged
into one LALR item, the LALR lookahead is the union of the lookaheads
of the LR(1) items.
For example, these two LR(1) states:

##### E→. E + T{$+}

##### E→. T {$+}

##### E→. E + T{)+}

##### E→. T {)+}

```
Would be merged into this single LALR state:
```
##### E→. E + T{$)+}

##### E→. T {$)+}

The resulting LALR automaton has the same number of states as the
LR(0) automaton, but has more precise lookahead information available
for each item. While this may seem a minor distinction, experience has
shown this simple improvement to be highly effective at obtaining the ef-
ficiency of SLR parsing while accommodating a large number of practical
grammars.

### 4.5 Grammar Classes Revisited

Now that you have some experience working with different kinds of gram-
mars, let’s step back and review how they relate to each other.

LL(1)⊂SLR⊂LALR⊂LR(1)⊂CFG (4.1)
**CFG:** A context-free grammar is any grammar whose rules have the
form A→α. To parse any CFG, we require a finite automaton (a parse
table) and a stack to keep track of the parse state. An arbitrary CFG can be
ambiguous. An ambiguous CFG will result in a non-deterministic finite


##### 4.6. THE CHOMSKY HIERARCHY 63

automaton, which is not practical to use. Instead, it is more desirable to
re-write the grammar to fit a more restricted class.
**LR(k):** An LR(k) parser performs a bottom-up **L** eft to right scan of the
input and provides a **R** ight-most parse, deciding what rule to apply next
by examining the nextktokens on the input. A canonical LR(1) parser
requires a very large finite automaton, because the possible lookaheads
are encoded into the states. While strictly a subset of CFGs, nearly all real-
world language constructs can be expressed adequately in LR(1).
**LALR:** A Lookahead-LR parser is created by first constructing a canon-
ical LR(1) parser, and then merging all itemsets that have the same core.
This yields a much smaller finite automaton, while retaining some detailed
lookahead information. While less powerful than canonical LR(1) in the-
ory, LALR is usually sufficient to express real-world languages.
**SLR:** A Simple-LR parser approximates an LR(1) parser by construct-
ing the LR(0) state machine, and then relying on the FIRSTand FOLLOW
sets to select which rule to apply. SLR is simple and compact, but there are
easy-to-find examples of common constructs that it cannot parse.
**LL(k):** An LL(k) parser performs a top-down **L** eft to right scan of the
input and provides a **L** eft-most parse, deciding what rule to apply next by
examining the nextktokens on the input. LL(1) parsers are simple and
widely used because they require a table that is onlyO(nt)wheretis the
number of tokens, andnis the number of non-terminals. LL(k)parsers
are less practical fork > 1 because the size of the parse table isO(ntk)in
the worst case.^2 However, they often require that a grammar be rewritten
to be more amenable to the parser, and are not able to express all common
language structures.

### 4.6 The Chomsky Hierarchy

Finally, this brings us to a fundamental result in theoretical computer sci-
ence, known as the **Chomsky hierarchy** [1], named after noted linguist
Noam Chomsky. The hierarchy describes four categories of languages
(and corresponding grammars) and relates them to the abstract computing
machinery necessary to recognize such a language.
**Regular languages** are those described by regular expressions, as you
learned back in Chapter 3. Every regular expression corresponds to a fi-
nite automaton that can be used to identify all words in the corresponding
language. As you know, a finite automaton can be implemented with the
very simple mechanism of a table and a single integer to represent the cur-
rent state. So, a scanner for a regular language is very easy to implement
efficiently.
**Context free languages** are those described by context free grammars
where each rule is of the form A→γ, with a single non-terminal on the

(^2) For example, an LL(1) parser would require a row for terminals{a,b,c,...}, while an
LL(2) parser would require a row for pairs{aa, ab, ac, ...}.


##### 64 CHAPTER 4. PARSING

```
Language Class Machine Required
Regular Languages Finite Automata
Context Free Languages Pushdown Automata
Context Sensitive Languages Linear Bounded Automata
Recursively Enumerable Languages Turing Machine
```
```
Figure 4.7: The Chomsky Hierarchy
```
left hand side, and a mix of terminals and non-terminals on the right hand
side. We call these “context free” because the meaning of a non-terminal is
the same in all places where it appears. As you have learned in this chap-
ter, a CFG requires a pushdown automaton, which is achieved by coupling
a finite automaton with a stack. If the grammar is ambiguous, the automa-
ton will be non-deterministic and therefore impractical. In practice, we
restrict ourselves to using subsets of CFGs (like LL(1) and LR(1) that are
non-ambiguous and result in a deterministic automaton that completes in
bounded time.
**Context sensitive languages** are those described by context sensitive
grammars where each rule can be of the formαAβ→αγβ. We call these
“context sensitive” because the interpretation of a non-terminal is con-
trolled by context in which it appears. Context sensitive languages re-
quire a non-deterministic linear bounded automaton, which is bounded
in memory consumption, but not in execution time. Context sensitive lan-
guages are not very practical for computer languages.
**Recursively enumerable languages** are the least restrictive set of lan-
guages, described by rules of the formα→βwhereαandβcan be any
combination of terminals and non-terminals. These languages can only be
recognized by a full Turing machine, and are the least practical of all.

The Chomsky Hierarchy is a specific example of a more general prin-
ciple for the design of languages and compilers:

**The least powerful language gives the strongest guarantees.**
That is to say, if we have a problem to be solved, it should be attacked
using the least expressive tool that is capable of addressing the problem. If
wecansolve a given problem by employing REs instead of CFGs, then we
shoulduse REs, because they consume less state, have simpler machinery,
and present fewer roadblocks to a solution.
The same advice applies more broadly: assembly language is the most
powerful language available in our toolbox and is capable of expressing
any program that the computer is capable of executing. However, assem-
bly language is also the most difficult to use because it gives none of the
guarantees found in higher level languages. Higher level languages are
lesspowerful than assembly language, and this is what makes them more
predictable, reliable, and congenial to use.


##### 4.7. EXERCISES 65

### 4.7 Exercises

1. Write out an improvement to GrammarG 5 that does not have the
    dangling-else problem. Hint: Prevent the innerSfrom containing
    anifwithout anelse.
2. Write a grammar for an interesting subset of sentences in English, in-
    cluding nouns, verbs, adjectives, adverbs, conjunctions, subordinate
    phrases, and so forth. (Include just a few terminals in each category
    to give the idea.) Is the grammar LL(1), LR(1), or ambiguous? Ex-
    plain why.
3. Consider the following grammar:

```
Grammar G 12
```
1. P→S
2. P→S P
3. S→if E then S
4. S→if E then S else S
5. S→while E S
6. S→begin P end
7. S→print E
8. S→E
9. E→id
10. E→integer
11. E→E + E

```
(a) Point out all aspects of GrammarG 12 which are not LL(1).
(b) Write a new grammar which accepts the same language, but
avoids left recursion and common left prefixes.
(c) Write the FIRST and FOLLOW sets for the new grammar.
(d) Write out the LL(1) parse table for the new grammar.
(e) Is the new grammar an LL(1) grammar? Explain your answer
carefully.
```
4. Consider the following grammar:

```
Grammar G 13
```
1. S→id = E
2. E→E + P
3. E→P
4. P→id
5. P→(E)
6. P→id(E)


##### 66 CHAPTER 4. PARSING

```
(a) Draw the LR(0) automaton for GrammarG 13.
(b) Write out the complete SLR parsing table for GrammarG 13.
(c) Is this grammar LL(1)? Explain why.
(d) Is this grammar SLR? Explain why.
```
5. Consider GrammarG 11 , shown earlier.

```
(a) Write out the complete LR(1) automaton for GrammarG 11.
(b) Compact the LR(1) automaton into the LALR automaton for
GrammarG 11.
```
6. Write a context free grammar that describes formal regular expres-
    sions. Start by writing out the simplest (possibly ambiguous) gram-
    mar you can think of, based on the inductive definition in Chapter 3.
    Then, rewrite the grammar into an equivalent LL(1) grammar.
7. (a) Write a grammar for the JSON data representation language.
    (b) Write the FIRSTand FOLLOWsets for your grammar.
       (c) Is your grammar LL(1), SLR, or LR(1), or neither? If necessary,
          re-write it until it is in the simplest grammar class possible.
(d) Write out the appropriate parse table for your grammar.
8. Write a working hand-coded parser for JSON expressions, making
    use of the JSON scanner constructed in the previous chapter.
9. Create a hand-coded scanner and a recursive descent parser that can
    evaluate first order logic expressions entered on the console. Include
    boolean values (T/F) and the operators&(and),|(or),!(not),->
    (implication), and()(grouping).
    For example, these expressions should evaluate to true:

```
T
T & T | F
( F -> F ) -> T
```
```
And these expressions should evaluate to false:
```
```
F
! ( T | F )
( T -> F ) & T
```
10. Write a hand-coded parser that reads in regular expressions and out-
    puts the corresponding NFA, using the Graphviz[2]DOT language.
11. Write a parser-construction tool that reads in an LL(1) grammar and
    produces working code for a table-driven parser as output.


##### 4.8. FURTHER READING 67

### 4.8 Further Reading

1. N. Chomsky, “On certain formal properties of grammars”, Informa-
    tion and Control, volume 2, number 2, 1959.
    [http://dx.doi.org/10.1016/S0019-9958(59)90362-6](http://dx.doi.org/10.1016/S0019-9958(59)90362-6)
2. J. Ellson, E. Gansner, L. Koutsofios, S. North, G. Woodhull, “Graphviz
    - Open Source Graph Drawing Tools”, International Symposium on
    Graph Drawing, 2001.
    [http://www.graphviz.org](http://www.graphviz.org)
3. J. Earley, “An Efficient Context Free Parsing Algorithm”, Communi-
    cations of the ACM, volume 13, issue 2, 1970.
    https://doi.org/10.1145/362007.362035
4. M. Tomita (editor), “Generalized LR Parsing”, Springer, 1991.


##### 68 CHAPTER 4. PARSING


##### 69

## Chapter 5 – Parsing in Practice

In this chapter, you will apply what you have learned about the theory of
LL(1) and LR(1) grammars in order to build a working parser for a simple
expression language. This will give you a basis to write a more complete
parser for B-Minor in the following chapter.
While LL(1) parsers are commonly written by hand, LR(1) parsers are
simply too cumbersome to do the same. Instead, we rely upon a **parser
generator** to take a specification of a grammar and automatically produce
the working code. In this chapter, we will give examples with Bison, a
widely-used parser generator for C like languages.
Using Bison, we will define an LALR grammar for algebraic expres-
sions, and then employ it to create three different varieties of programs.

- A **validator** reads the input program and then simply informs the
    user whether it is a valid sentence in the language specified by the
    grammar. Such a tool is often used to determine whether a given
    program conforms to one standard or another.
- An **interpreter** reads the input program and then actually executes
    the program to produce a result. One approach to interpretation is
    to compute the result of each operation as soon as it is parsed. The
    alternate approach is to parse the program into an abstract syntax
    tree, and then execute it.
- A **translator** reads the input program, parses it into an abstract syn-
    tax tree, and then traverses the abstract syntax tree to produce an
    equivalent program in a different format.


##### 70 CHAPTER 5. PARSING IN PRACTICE

### 5.1 The Bison Parser Generator

It is not practical to implement an LALR parser by hand, and so we rely on
tools to automatically generate tables and code from a grammar specifica-
tion. YACC (Yet Another Compiler Compiler) was a widely used parser
generator in the Unix environment, recently supplanted by the GNU Bison
parser which is generally compatible. Bison is designed to automatically
invoke Flex as needed, so it is easy to combine the two into a complete
program.
Just as with the scanner, we must create a specification of the grammar
to be parsed, where each rule is accompanied by an action to follow. The
overall structure of a Bison file is similar to that of Flex:

%{
(C preamble code)
%}
(declarations)
%%
(grammar rules)
%%
(C postamble code)

The first section contains arbitrary C code, typically#includestate-
ments and global declarations. The second section can contain a variety of
declarations specific to the Bison language. We will use the%tokenkey-
word to declare all of the terminals in our language. The main body of the
file contains a series of rules of the form

expr : expr TOKEN_ADD expr
| TOKEN_INT
;

indicating that non-terminalexprcan produce the sentence
expr TOKENADD expror the single terminalTOKENINT. White space
is not significant, so it’s ok to arrange the rules for clarity. Note that the
usual naming convention is reversed: since upper case is customarily used
for C constants, we use lower case to indicate non-terminals.
The resulting code creates a single functionyyparse()that returns
an integer: zero indicates a successful parse, one indicates a parse er-
ror, and two indicates an internal problem such as memory exhaustion.
yyparseassumes that there exists a functionyylexthat returns integer
token types. This can be written by hand or generated automatically by
Flex. In the latter case, the input source can be changed by modifying the
file pointeryyin.
Figure 5.1 gives a Bison specification for simple algebraic expressions
on integers. Remember that Bison accepts an LR(1) grammar, so it is ok to
have left recursion within the various rules.


##### 5.1. THE BISON PARSER GENERATOR 71

##### %{

#include <stdio.h>
%}

%token TOKEN_INT
%token TOKEN_PLUS
%token TOKEN_MINUS
%token TOKEN_MUL
%token TOKEN_DIV
%token TOKEN_LPAREN
%token TOKEN_RPAREN
%token TOKEN_SEMI
%token TOKEN_ERROR

%%
program : expr TOKEN_SEMI;

expr : expr TOKEN_PLUS term
| expr TOKEN_MINUS term
| term
;

term : term TOKEN_MUL factor
| term TOKEN_DIV factor
| factor
;

factor: TOKEN_MINUS factor
| TOKEN_LPAREN expr TOKEN_RPAREN
| TOKEN_INT
;
%%

int yywrap() { return 0; }

```
Figure 5.1: Bison Specification for Expression Validator
```

##### 72 CHAPTER 5. PARSING IN PRACTICE

#include <stdio.h>

extern int yyparse();

int main()
{
if(yyparse()==0) {
printf("Parse successful!\n");
} else {
printf("Parse failed.\n");
}
}

```
Figure 5.2: Main Program for Expression Validator
```
Figure 5.3 shows the general build procedure for a combined program
that uses Bison and Flex together. The parser specification goes in
parser.bison. We assume that you have written a suitable scanner
and placed it inscanner.flex. Previously, we wrotetoken.hby hand.
Here, we will rely on Bison to generatetoken.hautomatically from the
%tokendeclarations, so that the parser and the scanner are working from
the same information. Invoke Bison like this:

bison --defines=token.h --output=parser.c parser.bison

The--output=parser.coption directs Bison to write its code into
the fileparser.cinstead of the crypticyy.tab.c. Then, we compile
parser.c, thescanner.cgenerated by Flex, andmain.cindependently,
and link them together into a complete executable.

```
Compiler
```
```
scanner.o
```
```
Compiler main.o
```
```
Compiler
parser.o
```
```
Flex scanner.c
```
```
Bison
```
```
token.h
```
```
parser.c
```
```
Linker compiler.exe
```
```
scanner.flex
```
```
parser.bison
```
```
main.c
```
```
Figure 5.3: Build Procedure for Bison and Flex Together
```

##### 5.2. EXPRESSION VALIDATOR 73

If you give Bison the-voption, it will output a text representation of
the LALR automaton to the fileparser.output. For each state, it gives
the items, using dot to indicate the parser position. Then, it lists the actions
applied to that state. For example, suppose that we modify the grammar
above so that it becomes ambiguous:

expr : expr TOKEN_PLUS expr

Bison will report that the grammar has one shift-reduce conflict, and
parser.outputwill describe each state. In the event of a conflict, Bison
will suppress one or more actions, and this is indicated by square brackets
in the following report:

state 9
2 expr: expr. TOKEN_PLUS expr
2 | expr TOKEN_PLUS expr.

```
TOKEN_PLUS shift, and go to state 7
TOKEN_PLUS [reduce using rule 2 (expr)]
$default reduce using rule 2 (expr)
```
Be careful! If your grammar has shift-reduce or reduce-reduce con-
flicts, Bison will happily output working code with some of the conflicting
actions suppressed. The code may appear to work on simple inputs, but
is likely to have unexpected effects on complete programs. Always check
for conflicts before proceeding.

### 5.2 Expression Validator

As written, the Bison specification in Figure 5.1 will simply evaluate whether
the input program matches the desired grammar.yyparse()will return
zero on success, and non-zero otherwise. Such a program is known as a
**validator** and is often used to determine whether a given program is stan-
dards compliant.
There are a variety of online validators for web-related languages like
HTML^1 , CSS^2 , and JSON^3. By having a strict language definition separate
from actual implementations (which may contain non-standard features)
it is much easier for a programmer to determine whether their code is
standards compliant, and therefore (presumably) portable.

(^1) [http://validator.w3.org](http://validator.w3.org)
(^2) [http://css-validator.org](http://css-validator.org)
(^3) [http://jsonlint.com](http://jsonlint.com)


##### 74 CHAPTER 5. PARSING IN PRACTICE

### 5.3 Expression Interpreter

To do more than simply validate the program, we must make use of **se-
mantic actions** embedded within the grammar itself. Following the right
side of any production rule, you may place arbitrary C code inside of curly
braces. This code may refer to **semantic values** which represent the values
already computed for other non-terminals. Semantic values are given by
dollar signs indicating the position of a non-terminal in a production rule.
Two dollar signs indicates the semantic value of the current rule.
For example, in the rule for addition, the appropriate semantic action is
to add the left value (the first symbol) to the right value (the third symbol):

expr : expr TOKEN_PLUS term { $$ = $1 + $3; }

Where do the semantic values$1and$3come from? They simply
come from the other rules that define those non-terminals. Eventually, we
reach a rule that gives the value for a leaf node. For example, this rule
indicates that the semantic value of an integer token is the integer value of
the token text:

factor : TOKEN_INT { $$ = atoi(yytext); }

(Careful: the value of the token comes from theyytextarray in the
scanner, so you can only do this when the rule has a single terminal on the
right hand side of the rule.)
In the cases where a non-terminal expands to a single non-terminal, we
simply assign one semantic value to the other:

term : factor { $$ = $1; }

Because Bison is a bottom-up parser, it determines the semantic values
of the leaf nodes in the parse tree first, then passes those up to the interior
nodes, and so on until the result reaches the start symbol.
Figure 5.4 shows a Bison grammar that implements a complete inter-
preter. The main program simply invokesyyparse(). If successful, the
result is stored in the global variableparserresultfor extraction and
use from the main program.


##### 5.4. EXPRESSION TREES 75

prog : expr TOKEN_SEMI { parser_result = $1; }
;

expr : expr TOKEN_PLUS term { $$ = $1 + $3; }
| expr TOKEN_MINUS term { $$ = $1 - $3; }
| term { $$ = $1; }
;

term : term TOKEN_MUL factor { $$ = $1 * $3; }
| term TOKEN_DIV factor { $$ = $1 / $3; }
| factor { $$ = $1; }
;

factor
: TOKEN_MINUS factor { $$ = -$2; }
| TOKEN_LPAREN expr TOKEN_RPAREN { $$ = $2; }
| TOKEN_INT { $$ = atoi(yytext); }
;

```
Figure 5.4: Bison Specification for an Interpreter
```
### 5.4 Expression Trees

So far, our expression interpreter is computing results in the middle of
parsing the input. While this works for simple expressions, it has several
general drawbacks: One is that the program may perform a large amount
of computation, only to discover a parse error late in the program. It is
generally more desirable to find all parse errorsbeforeexecution.
To fix this, we will add a new stage to the interpreter. Instead of com-
puting values outright, we will construct a data structure known as the
**abstract syntax tree** to represent the expression. Once the AST is created,
we can traverse the tree to check, execute, and translate the program as
needed.
Figure 5.5 shows the C code for a simple AST representing expressions.
exprtenumerates the five kinds of expression nodes.struct exprde-
scribes a node in the tree, which is described by a kind, a left and right
pointer, and an integervaluefor a leaf. The functionexprcreatecre-
ates a new tree node of any kind, whileexprcreatevaluecreates one
specifically of kindEXPRVALUE.^4

(^4) Although it is verbally awkward, we are using the term “kind” rather than “type”, which
will have a very specific meaning later on.


##### 76 CHAPTER 5. PARSING IN PRACTICE

**Contents of File: expr.h**

typedef enum {
EXPR_ADD,
EXPR_SUBTRACT,
EXPR_DIVIDE,
EXPR_MULTIPLY,
EXPR_VALUE
} expr_t;

```
struct expr {
expr_t kind;
struct expr *left;
struct expr *right;
int value;
};
```
**Contents of File: expr.c**

struct expr * expr_create( expr_t kind,
struct expr *left,
struct expr *right )
{
struct expr *e = malloc(sizeof(*e));
e->kind = kind;
e->value = 0;
e->left = left;
e->right = right;
return e;
}

struct expr * expr_create_value( int value )
{
struct expr *e = expr_create(EXPR_VALUE,0,0);
e->value = value;
return e;
}

```
Figure 5.5: AST for Expression Interpreter
```

##### 5.4. EXPRESSION TREES 77

Using the expression structure, we can create some simple ASTs by
hand. For example, if we wanted to create an AST corresponding to the
expression(10+20)* 30 , we could issue this sequence of operations:

struct expr *a = expr_create_value(10);
struct expr *b = expr_create_value(20);
struct expr *c = expr_create(EXPR_ADD,a,b);
struct expr *d = expr_create_value(30);
struct expr *e = expr_create(EXPR_MULTIPLY,c,d);

Of course, we could have accomplished the same thing by writing a
single expression with nested values:

struct expr *e =
expr_create(EXPR_MULTIPLY,
expr_create(EXPR_ADD,
expr_create_value(10),
expr_create_value(20)
),
expr_create_value(30)
);

```
Either way, the result is a data structure like this:
```
```
VALUE: 10 VALUE: 20
```
```
ADD VALUE: 30
```
```
MULTIPLY
```
```
e
```
Instead of building each node of the AST by hand, we want Bison to
do the same work automatically. As each element of an expression is rec-
ognized, a new node in the tree should be created, and passed up so that
it can be linked into the appropriate place. By doing a bottom-up parse,
Bison will create the leaves of the tree first, and then link them into the
parent nodes.
To accomplish this, we must write the semantic actions for each rule to
either create a node in the tree, or pass up the pointer from the node below.
Figure 5.6 shows how this is done:


##### 78 CHAPTER 5. PARSING IN PRACTICE

##### %{

#include "expr.h"
#define YYSTYPE struct expr *
struct expr * parser_result = 0;
%}

/* token definitions omitted for brevity */

prog : expr TOKEN_SEMI
{ parser_result = $1; }
;

expr : expr TOKEN_PLUS term
{ $$ = expr_create(EXPR_ADD,$1,$3); }
| expr TOKEN_MINUS term
{ $$ = expr_create(EXPR_SUBTRACT,$1,$3); }
| term
{ $$ = $1; }
;

term : term TOKEN_MUL factor
{ $$ = expr_create(EXPR_MULTIPLY,$1,$3); }
| term TOKEN_DIV factor
{ $$ = expr_create(EXPR_DIVIDE,$1,$3); }
| factor
{ $$ = $1; }
;

factor
: TOKEN_MINUS factor
{ $$ = expr_create(EXPR_SUBTRACT,
expr_create_value(0),$2); }
| TOKEN_LPAREN expr TOKEN_RPAREN
{ $$ = $2; }
| TOKEN_INT
{ $$ = expr_create_value(atoi(yytext));
;

```
Figure 5.6: Building an AST for the Expression Grammar
```

##### 5.4. EXPRESSION TREES 79

```
Examine Figure 5.6 carefully and note several things:
```
- In the preamble, we must explicitly define the **semantic type** by set-
    ting the macro **YYSTYPE** tostruct expr*. This causes Bison
    to usestruct expr *as the internal type everywhere a semantic
    value such as$$or$1is used. The final parser result must have the
    same semantic type, of course.
- The AST does not always correspond directly to the parse tree. For
    example, where anexprproduces afactor, we simply pass up
    the pointer to the underlying node with{$$ = $1;}On the other
    hand, when we encounter a unary minus interm, we return a sub-
    tree that actually implements subtraction between the value zero on
    the left and the expression on the right.

```
Parse Tree for-20 AST for-20
expr
```
```
term
```
```
factor
MINUS
```
```
factor
20
```
```
SUB
```
```
VALUE:0 VALUE:20
```
- Parentheses are not directly represented in the AST. Instead, they
    have the effect of ordering the nodes in the tree to achieve the desired
    evaluation order. For example, consider the AST generated by these
    sentences:

(5+6)/7 5+(6/7)

```
VALUE: 5 VALUE: 6
```
```
ADD VALUE: 7
```
```
DIV
```
```
VALUE: 5
```
```
VALUE: 6
```
```
ADD
```
```
DIV
```
```
VALUE: 7
```

##### 80 CHAPTER 5. PARSING IN PRACTICE

int expr_evaluate( struct expr *e )
{
if(!e) return 0;

```
int l = expr_evaluate(e->left);
int r = expr_evaluate(e->right);
```
```
switch(e->kind) {
case EXPR_VALUE: return e->value;
case EXPR_ADD: return l+r;
case EXPR_SUBTRACT: return l-r;
case EXPR_MULTIPLY: return l*r;
case EXPR_DIVIDE:
if(r==0) {
printf("error: divide by zero\n");
exit(1);
}
return l/r;
}
```
return 0;
}

```
Figure 5.7: Evaluating Expressions
```
Now that we have constructed the AST, we can use it as the basis for
computation and many other operations.
The AST can be evaluated arithmetically by callingexprevaluate
shown in Figure 5.7. This function performs a post-order traversal of the
tree by invoking itself recursively on the left and right pointers of the node.
(Note the check for a null pointer at the beginning of the function.) Those
calls returnlandrwhich contain the integer result of the left and right
subtrees. Then, the result of this node is computed by switching on the
kindof the current node. (Note also that we must check for division-by-
zero explicitly, otherwiseexprevaluatewould crash whenris zero.)


##### 5.5. EXERCISES 81

void expr_print( struct expr *e )
{
if(!e) return;

```
printf("(");
expr_print(e->left);
```
```
switch(e->kind) {
case EXPR_VALUE: printf("%d",e->value);
break;
case EXPR_ADD: printf("+"); break;
case EXPR_SUBTRACT: printf("-"); break;
case EXPR_MULTIPLY: printf("*"); break;
case EXPR_DIVIDE: printf("/"); break;
}
```
expr_print(e->right);
printf(")");
}

```
Figure 5.8: Printing and Evaluating Expressions
```
In a similar way, the AST can be converted back to text by calling
exprprint, shown in Figure 5.8. This function performs an in-order
traversal of the expression tree by recursively callingexprprinton the
left side of a node, displaying the current node, then callingexprprint
on the right side. Again, note the test for null at the beginning of the func-
tion.
As noted earlier, parentheses are not directly reflected in the AST. To
be conservative, this function displays a parenthesis around every value.
While correct, it results in a lot of parentheses! A better solution would be
to only print a parenthesis when a subtree contains an operator of lower
precedence.

### 5.5 Exercises

1. Consult the Bison manual and determine how to automatically gen-
    erate a graph of the LALR automaton from your grammar. Compare
    the output from Bison against your hand-drawn version.
2. Modifyexprevaluate()(and anything else needed) to handle
    floating point values instead of integers.
3. Modifyexprprint()so that it displays the minimum number of
    parentheses necessary for correctness.


##### 82 CHAPTER 5. PARSING IN PRACTICE

4. Extend the parser and interpreter to allow for invoking several built-
    in mathematical functions such assin(x),sqrt(x)and so forth.
    Before coding, think a little bit about where to put the names of the
    functions. Should they be keywords in the language? Or should
    any function names be simply treated as identifiers and checked in
    exprevaluate()?
5. Extend the parser and interpreter to allow for variable assignment
    and use, so that you can write multiple assignment statements fol-
    lowed by a single expression to be evaluated, like this:

```
g = 9.8;
t = 5;
g*t*t - 7*t + 10;
```

##### 5.6. FURTHER READING 83

### 5.6 Further Reading

As its name suggests, YACC was not the first compiler construction tool,
but it remains widely used today and has led to a proliferation of simi-
lar tools written in various languages and addressing different classes of
grammars. Here is just a small selection:

1. S. C. Johnson, “YACC: Yet Another Compiler-Compiler”, Bell Labo-
    ratories Technical Journal, 1975.
2. D. Grune and C.J.H Jacobs, “A programmer-friendly LL(1) parser
    generator”, Software: Practice and Experience, volume 18, number
    1.
3. T.J. Parr and R.W. Quong, “ANTLR: A predicated LL(k) Parser Gen-
    erator”, Software: Practice and Experience, 1995.
4. S. McPeak, G.C. Necula, “Elkhound: A Fast, Practical GLR Parser
    Generator”, International Conference on Compiler Construction, 2004.


##### 84 CHAPTER 5. PARSING IN PRACTICE


##### 85

## Chapter 6 – The Abstract Syntax Tree

### 6.1 Overview

The **abstract syntax tree (AST)** is an important internal data structure that
represents the primary structure of a program. The AST is the starting
point for semantic analysis of a program. It is “abstract” in the sense that
the structure leaves out the particular details of parsing: the AST does
not care whether a language has prefix, postfix, or infix expressions. (In
fact, the AST we describe here can be used to represent most procedural
languages.)
For our project compiler, we will define an AST in terms of five C
structures representing declarations, statements, expressions, types, and
parameters. While you have certainly encountered each of these terms
while learning programming, they are not always used precisely in prac-
tice. This chapter will help you to sort those items out very clearly:

- A **declaration** states the name, type, and value of a symbol so that
    it can be used in the program. Symbols include items such as con-
    stants, variables, and functions.
- A **statement** indicates an action to be carried out that changes the
    state of the program. Examples include loops, conditionals, and
    function returns.
- An **expression** is a combination of values and operations that is **eval-**
    **uated** according to specific rules and yields a **value** such as an inte-
    ger, floating point, or string. In some programming languages, an
    expression may also have a **side effect** that changes the state of the
    program.

For each kind of element in the AST, we will give an example of the
code and how it is constructed. Because each of these structures poten-
tially has pointers to each of the other types, it is necessary to preview all
of them before seeing how they work together.
Once you understand all of the elements of the AST, we finish the chap-
ter by demonstrating how the entire structure can be created automatically
through the use of the Bison parser generator.


##### 86 CHAPTER 6. THE ABSTRACT SYNTAX TREE

### 6.2 Declarations

A complete B-Minor program is a sequence of declarations. Each declara-
tion states the existence of a variable or a function. A variable declaration
may optionally give an initializing value. If none is given, it is given a de-
fault value of zero. A function declaration may optionally give the body
of the function in code; if no body is given, then the declaration serves as
a prototype for a function declared elsewhere.
For example, the following are all valid declarations:

b: boolean;
s: string = "hello";
f: function integer ( x: integer ) = { return x*x; }

A declaration is represented by adeclstructure that gives the name,
type, value (if an expression), code (if a function), and a pointer to the next
declaration in the program:

struct decl {
char *name;
struct type *type;
struct expr *value;
struct stmt *code;
struct decl *next;
};

Because we will be creating a lot of these structures, you will need a
factory function that allocates a structure and initializes its fields, like this:

struct decl * decl_create( char*name,
struct type *type,
struct expr *value,
struct stmt *code,
struct decl *next )
{
struct decl *d = malloc(sizeof(*d));
d->name = name;
d->type = type;
d->value = value;
d->code = code;
d->next = next;
return d;
}

(You will need to write similar code for statements, expressions, etc,
but we won’t keep repeating it here.)


##### 6.2. DECLARATIONS 87

The three declarations on the preceding page can be represented graph-
ically as a linked list, like this:

```
struct decl
name type value code next
```
```
b boolean
```
```
struct decl
name type value code next
```
```
s string "hello"
```
```
struct decl
name type value code next
```
```
f
```
```
function(x:integer) returns integer
```
```
return x*x;
```
Note that some of the fields point to nothing: these would be repre-
sented by a null pointer, which we omit for clarity. Also, our picture is
incomplete and must be expanded: the items representing types, expres-
sions, and statements are all complex structures themselves that we must
describe.


##### 88 CHAPTER 6. THE ABSTRACT SYNTAX TREE

### 6.3 Statements

The body of a function consists of a sequence of statements. A statement
indicates that the program is to take a particular action in the order speci-
fied, such as computing a value, performing a loop, or choosing between
branches of an alternative. A statement can also be a declaration of a local
variable. Here is thestmtstructure:

struct stmt {
stmt_t kind;
struct decl *decl;
struct expr *init_expr;
struct expr *expr;
struct expr *next_expr;
struct stmt *body;
struct stmt *else_body;
struct stmt *next;
};

```
typedef enum {
STMT_DECL,
STMT_EXPR,
STMT_IF_ELSE,
STMT_FOR,
STMT_PRINT,
STMT_RETURN,
STMT_BLOCK
} stmt_t;
```
```
Thekindfield indicates what kind of statement it is:
```
- STMTDECLindicates a (local) declaration, and thedeclfield will
    point to it.
- STMTEXPRindicates an expression statement and theexprfield
    will point to it.
- STMTIFELSEindicates an if-else expression such that theexpr
    field will point to the control expression, thebodyfield to the state-
    ments executed if it is true, and theelsebodyfield to the state-
    ments executed if it is false.
- STMTFORindicates a for-loop, such thatinitexpr,expr, and
    nextexprare the three expressions in the loop header, andbody
    points to the statements in the loop.
- STMTPRINTindicates aprintstatement, andexprpoints to the
    expressions to print.
- STMTRETURNindicates areturnstatement, andexprpoints to the
    expression to return.
- STMTBLOCKindicates a block of statements inside curly braces, and
    bodypoints to the contained statements.


##### 6.3. STATEMENTS 89

And, as we did with declarations, we require a functionstmtcreate
to create and return a statement structure:

struct stmt * stmt_create( stmt_t kind,
struct decl *decl, struct expr*init_expr,
struct expr *expr, struct expr*next_expr,
struct stmt *body, struct stmt*else_body,
struct stmt *next );

This structure has a lot of fields, but each one serves a purpose and is
used when necessary for a particular kind of statement. For example, an
if-else statement only uses theexpr,body, andelsebodyfields, leaving
the rest null:

```
if( x<y ) print x; else print y;
```
```
STMT_IF_ELSE
decl init_expr expr next_expr body else_body next
```
```
x<y print x; print y;
```
A for-loop uses the threeexprfields to represent the three parts of the
loop control, and thebodyfield to represent the code being executed:

```
for(i=0;i<10;i++) print i;
```
```
STMT_FOR
decl init_expr expr next_expr body else_body next
```
```
i=0 i<10 i++ print i;
```

##### 90 CHAPTER 6. THE ABSTRACT SYNTAX TREE

### 6.4 Expressions

Expressions are implemented much like the simple expression AST shown
in Chapter 5. The difference is that we need many more binary types: one
for every operator in the language, including arithmetic, logical, compar-
ison, assignment, and so forth. We also need one for every type of leaf
value, including variable names, constant values, and so forth. Thename
field will be set forEXPRNAME, theintegervaluefield for
EXPRINTEGERLITERAL, and so on. You may need to add values and
types to this structure as you expand your compiler.

struct expr {
expr_t kind;
struct expr *left;
struct expr *right;

const char *name;
int integer_value;
const char *
string_literal;
};

```
typedef enum {
EXPR_ADD,
EXPR_SUB,
EXPR_MUL,
EXPR_DIV,
...
EXPR_NAME,
EXPR_INTEGER_LITERAL,
EXPR_STRING_LITERAL
} expr_t;
```
As before, you should create a factory for a binary operator:

struct expr * expr_create( expr_t kind,
struct expr *L,
struct expr *R );

And then a factory for each of the leaf types:

struct expr * expr_create_name( const char *name );
struct expr * expr_create_integer_literal( int i );
struct expr * expr_create_boolean_literal( int b );
struct expr * expr_create_char_literal( char c );
struct expr * expr_create_string_literal
( const char *str );

Note that you can store the integer, boolean, and character literal val-
ues all in theintegervaluefield.


##### 6.4. EXPRESSIONS 91

A few cases deserve special mention. Unary operators like logical-not
typically have their sole argument in theleftpointer:

## !x

```
EXPR_NOT
left right
```
```
EXPR_NAME
x
```
A function call is constructed by creating anEXPRCALLnode, such
that the left-hand side is the function name, and the right hand side is an
unbalanced tree ofEXPRARGnodes. While this looks a bit awkward, it al-
lows us to express a linked list using a tree, and will simplify the handling
of function call arguments on the stack during code generation.

## f(a,b,c)

```
EXPR_CALL
left right
```
```
EXPR_NAME
f
```
```
EXPR_ARG
left right
```
```
EXPR_NAME
a
```
```
EXPR_ARG
left right
```
```
EXPR_NAME
b
```
```
EXPR_ARG
left right
```
```
EXPR_NAME
c
```

##### 92 CHAPTER 6. THE ABSTRACT SYNTAX TREE

Array subscripting is treated like a binary operator, such that the name
of the array is on the left side of theEXPRSUBSCRIPToperator, and an
integer expression on the right:

## a[b]

```
EXPR_SUBSCRIPT
left right
```
```
EXPR_NAME
a
```
```
EXPR_NAME
b
```
### 6.5 Types

Atypestructure encodes the type of every variable and function men-
tioned in a declaration. Primitive types likeintegerandbooleanare
expressed by simply setting thekindfield appropriately, and leaving the
other fields null. Compound types likearrayandfunctionare built by
connecting multipletypestructures together.

typedef enum {
TYPE_VOID,
TYPE_BOOLEAN,
TYPE_CHARACTER,
TYPE_INTEGER,
TYPE_STRING,
TYPE_ARRAY,
TYPE_FUNCTION
} type_t;

struct type {
type_t kind;
struct type *subtype;
struct param_list *params;
};

struct param_list {
char *name;
struct type *type;
struct param_list *next;
};


##### 6.5. TYPES 93

For example, to express a basic type like a boolean or an integer, we
simply create a standalonetypestructure, withkindset appropriately,
and the other fields null:

## boolean

```
TYPE_BOOLEAN
subtype param
```
## integer

```
TYPE_INTEGER
subtype param
```
To express a compound type like an array of integers, we setkindto
TYPEARRAYand setsubtypeto point to aTYPEINTEGER:

```
array [] integer
```
```
TYPE_ARRAY
subtype param
```
```
TYPE_INTEGER
subtype param
```
These can be linked to arbitrary depth, so to express an array of array
of integers:

```
array [] array [] integer
```
```
TYPE_ARRAY
subtype param
```
```
TYPE_ARRAY
subtype param
```
```
TYPE_INTEGER
subtype param
```

##### 94 CHAPTER 6. THE ABSTRACT SYNTAX TREE

To express the type of a function, we usesubtypeto express the return
type of the function, and then connect a linked list ofparamlistnodes
to describe the name and type of each parameter to the function.
For example, here is the type of a function which takes two arguments
and returns an integer:

```
function integer (s:string, c:char)
```
```
TYPE_FUNCTION
subtype param
```
```
TYPE_INTEGER
subtype param
```
```
struct param_list
name type next
```
```
struct param_list
name type next
"s"
TYPE_STRING
subtype param
```
```
"c"
TYPE_CHAR
subtype param
```
Note that the type structures here let us express some complicated and
powerful higher order concepts of programming. By simply swapping in
complex types, you can describe an array of ten functions, each returning
an integer:

a: array [10] function integer ( x: integer );

```
Or how about a function that returns a function?
```
f: function function integer (x:integer) (y:integer);

```
Or even a function that returns an array of functions!
```
g: function array [10]
function integer (x:integer) (y:integer);

While the B-Minor type system is capable ofexpressingthese ideas,
these combinations will be rejected later in typechecking, because they re-
quire a more dynamic implementation than we are prepared to create. If
you find these ideas interesting, then you should read up on functional
languages such as Scheme and Haskell.


##### 6.6. PUTTING IT ALL TOGETHER 95

### 6.6 Putting it All Together

Now that you have seen each individual component, let’s see how a com-
plete B-Minor function would be expressed as an AST:

compute: function integer ( x:integer ) = {
i: integer;
total: integer = 0;
for(i=0;i<10;i++) {
total = total + i;
}
return total;
}

```
DECL "compute"
typevaluecodenext
TYPE_FUNCTION
subtypeparams
STMT_DECL
declinit_exprexprnext_exprbodyelse_bodynext
TYPE_INTEGER
subtypeparams
PARAM
nametypenext
DECL "i"
typevaluecodenext
STMT_DECL
declinit_exprexprnext_exprbodyelse_bodynext
"x" subtypeTYPE_INTEGERparams subtypeTYPE_INTEGERparams typevalueDECL "total"codenext
```
```
STMT_FOR
declinit_exprexprnext_exprbodyelse_bodynext
```
```
TYPE_INTEGER
subtypeparams
EXPR_INTEGER
0
```
```
EXPR_ASSIGN
left right
EXPR_LT
leftright
EXPR_INC
leftright
```
```
STMT_BLOCK
declinit_exprexprnext_exprbodyelse_bodynext
```
```
STMT_RETURN
declinit_exprexprnext_exprbodyelse_bodynext
```
```
EXPR_NAMEi EXPR_INTEGER 0 EXPR_NAMEi EXPR_INTEGER 10 EXPR_NAMEi
```
```
STMT_EXPR
declinit_exprexprnext_exprbodyelse_bodynext
```
```
EXPR_NAME
total
```
```
EXPR_ASSIGN
left right
EXPR_NAME
total
EXPR_ADD
leftright
EXPR_NAME
total
EXPR_NAME
i
```

##### 96 CHAPTER 6. THE ABSTRACT SYNTAX TREE

### 6.7 Building the AST

With the functions created so far in this chapter, we could, in principle,
construct the AST manually in a sort of nested style. For example, the fol-
lowing code represents a function calledsquarewhich accepts an integer
xas a parameter, and returns the valuex*x:

d = decl_create(
"square",
type_create(TYPE_FUNCTION,
type_create(TYPE_INTEGER,0,0),
param_list_create(
"x",
type_create(TYPE_INTEGER,0,0),
0)),
0,
stmt_create(STMT_RETURN,0,0,
expr_create(EXPR_MUL,
expr_create_name("x"),
expr_create_name("x")),
0,0,0,0),
0);

Obviously, this is no way to write code! Instead, we want our parser
to invoke the various creation functions whenever they are reduced, and
then hook them up into a complete tree. Using an LR parser generator
like Bison, this process is straightforward. (Here I will give you the idea
of how to proceed, but you will need to figure out many of the details in
order to complete the parser.)
At the top level, a B-Minor program is a sequence of declarations:

program : decl_list
{ parser_result = $1; }
;

Then, we write rules for each of the various kinds of declarations in a
B-Minor program:

decl : name TOKEN_COLON type TOKEN_SEMI
{ $$ = decl_create($1,$3,0,0,0); }
| name TOKEN_COLON type TOKEN_ASSIGN expr TOKEN_SEMI
{ $$ = decl_create($1,$3,$5,0,0); }
| /* and more cases here*/

...
;


##### 6.7. BUILDING THE AST 97

Since eachdeclstructure is created separately, we must connect them
together in a linked list formed by adecllist. This is most easily done
by making the rule right-recursive, so thatdeclon the left represents one
declaration, anddeclliston the right represents the remainder of the
linked list. The end of the list is a null value whendecllistproduces.

decl_list : decl decl_list
{ $$ = $1; $1->next = $2; }
| /* epsilon*/
{ $$ = 0; }
;

For each kind of statement, we create astmtstructure that pulls out
the necessary elements from the grammar.

stmt : TOKEN_IF TOKEN_LPAREN expr TOKEN_RPAREN stmt
{ $$ = stmt_create(STMT_IF_ELSE,0,0,$3,0,$5,0,0); }
| TOKEN_LBRACE stmt_list TOKEN_RBRACE
{ $$ = stmt_create(STMT_BLOCK,0,0,0,0,$2,0,0); }
| /* and more cases here*/

...
;

Proceed in this way down through each of the grammar elements of
a B-Minor program: declarations, statements, expressions, types, parame-
ters, until you reach the leaf elements of literal values and symbols, which
are handled in the same way as in Chapter 5.
There is one last complication: What, exactly is the semantic type of the
values returned as each rule is reduced? It isn’t a single type, because each
kind of rule returns a different data structure: a declaration rule returns a
struct decl *, while an identifier rule returns achar *. To make this
work, we inform Bison that the semantic value is the union of all of the
types in the AST:

%union {
struct decl *decl;
struct stmt *stmt;

...
char*name;
};

```
And then indicate the specific subfield of the union used by each rule:
```
%type <decl> program decl_list decl...
%type <stmt> stmt_list stmt...

...
%type <name> name


##### 98 CHAPTER 6. THE ABSTRACT SYNTAX TREE

### 6.8 Exercises

1. Write a complete LR grammar for B-Minor and test it using Bison.
    Your first attempt will certainly have many shift-reduce and reduce-
    reduce conflicts, so use your knowledge of grammars from Chapter 4
    to rewrite the grammar and eliminate the conflicts.
2. Write the AST structures and generating functions as outlined in
    this chapter, and manually construct some simple ASTs using nested
    function calls as shown above.
3. Add new functionsdeclprint(),stmtprint(), etc. that print
    the AST back out so you can verify that the program was generated
    correctly. Make your output nicely formatted using indentation and
    consistent spacing, so that the code is easily readable.
4. Add the AST generator functions as action rules to your Bison gram-
    mar, so that you can parse complete programs, and print them back
    out again.
5. Add new functionsdecltranslate(),stmttranslate(), etc.
    that output the B-Minor AST in a different language of your own
    choosing, such as Python or Java or Rust.
6. Add new functions that emit the AST in a graphical form so you can
    “see” the structure of a program. One approach would be to use the
    Graphviz DOT format: let each declaration, statement, etc be a node
    in a graph, and then let each pointer between structures be an edge
    in the graph.


##### 99

## Chapter 7 – Semantic Analysis

Now that we have completed construction of the AST, we are ready to
begin analyzing the **semantics** , or the actual meaning of a program, and
not simply its structure.
**Type checking** is a major component of semantic analysis. Broadly
speaking, the type system of a programming language gives the program-
mer a way to make verifiable assertions that the compiler can check auto-
matically. This allows for the detection of errors at compile-time, instead
of at runtime.
Different programming languages have different approaches to type
checking. Some languages (like C) have a rather weak type system, so it
is possible to make serious errors if you are not careful. Other languages
(like Ada) have very strong type systems, but this makes it more difficult
to write a program that will compile at all!
Before we can perform type checking, we must determine the type of
each identifier used in an expression. However, the mapping between
variable names and their actual storage locations is not immediately obvi-
ous. A variablexin an expression could refer to a local variable, a func-
tion parameter, a global variable, or something else entirely. We solve this
problem by performing **name resolution** , in which each definition of a
variable is entered into a **symbol table**. This table is referenced throughout
the semantic analysis stage whenever we need to evaluate the correctness
of some code.
Once name resolution is completed, we have all the information nec-
essary to check types. In this stage, we compute the type of complex ex-
pressions by combining the basic types of each value according to stan-
dard conversion rules. If a type is used in a way that is not permitted, the
compiler will output an (ideally helpful) error message that will assist the
programmer in resolving the problem.
Semantic analysis also includes other forms of checking the correctness
of a program, such as examining the limits of arrays, avoiding bad pointer
traversals, and examining control flow. Depending on the design of the
language, some of these problems can be detected at compile time, while
others may need to wait until runtime.


##### 100 CHAPTER 7. SEMANTIC ANALYSIS

### 7.1 Overview of Type Systems

Most programming languages assign to every value (whether a literal,
constant, or variable) a **type** , which describes the interpretation of the data
in that variable. The type indicates whether the value is an integer, a float-
ing point number, a boolean, a string, a pointer, or something else. In most
languages, these atomic types can be combined into higher-order types
such as enumerations, structures, and variant types to express complex
constraints.
The type system of a language serves several purposes:

- **Correctness.** A compiler uses type information provided by the pro-
    grammer to raise warnings or errors if a program attempts to do
    something improper. For example, it is almost certainly an error to
    assign an integer value to a pointer variable, even though both might
    be implemented as a single word in memory. A good type system
    can help to eliminate runtime errors by flagging them at compile
    time instead.
- **Performance.** A compiler can use type information to find the most
    efficient implementation of a piece of code. For example, if the pro-
    grammer tells the compiler that a given variable is a constant, then
    the same value can be loaded into a register and used many times,
    rather than constantly loading it from memory.
- **Expressiveness.** A program can be made more compact and expres-
    sive if the language allows the programmer to leave out facts that
    can be inferred from the type system. For example, in B-Minor the
    printstatement does not need to be told whether it is printing an
    integer, a string, or a boolean: the type is inferred from the expres-
    sion and the value is automatically displayed in the proper way.

A programming language (and its type system) are commonly classi-
fied on the following axes:

- safe or unsafe
- static or dynamic
- explicit or implicit

In an **unsafe programming language** , it is possible to write valid pro-
grams that have wildly undefined behavior that violates the basic struc-
ture of the program. For example, in the C programming language, a pro-
gram can construct an arbitrary pointer to modify any word in memory,
and thereby change the data and code of the compiled program. Such
power is probably necessary to implement low-level code like an operat-
ing system or a driver, but is problematic in general application code.


##### 7.1. OVERVIEW OF TYPE SYSTEMS 101

For example, the following code in C is syntactically legal and will
compile, but is unsafe because it writes data outside the bounds of the
arraya[]. As a result, the program could have almost any outcome, in-
cluding incorrect output, silent data corruption, or an infinite loop.

/* This is C code */
int i;
int a[10];
for(i=0;i<100;i++) a[i] = i;

In a **safe programming language** , it is not possible to write a program
that violates the basic structures of the language. That is, no matter what
input is given to a program written in a safe language, it will always
execute in a well defined way that preserves the abstractions of the lan-
guage. A safe programming language enforces the boundaries of arrays,
the use of pointers, and the assignment of types to prevent undefined be-
havior. Most interpreted languages, like Perl, Python, and Java, are safe
languages.
For example, in C#, the boundaries of arrays are checked at runtime, so
that running off the end of an array has the predictable effect of throwing
anIndexOutOfRangeException:

/* This is C-sharp code*/
a = new int[10];
for(int i=0;i<100;i++) a[i] = i;

In a **statically typed language** , all typechecking is performed at compile-
time, long before the program runs. This means that the program can be
translated into basic machine code without retaining any of the type in-
formation, because all operations have been checked and determined to
be safe. This yields the most high performance code, but does eliminate
some kinds of convenient programming idioms.
Static typing is often used to distinguish between integer and floating
point operations. While operations like addition and multiplication are
usually represented by the same symbols in the source language, they are
implemented with fundamentally different machine code. For example, in
the C language on X86 machines,(a+b)would be translated to anADDL
instruction for integers, but anFADDinstruction for floating point values.
To know which instruction to apply, we must first determine the type ofa
andband deduce the intended meaning of+.
In a **dynamically typed language** , type information is available at run-
time, and stored in memory alongside the data that it describes. As the
program executes, the safety of each operation is checked by comparing
the types of each operand. If types are observed to be incompatible, then
the program must halt with a runtime type error. This also allows for


##### 102 CHAPTER 7. SEMANTIC ANALYSIS

code that can explicitly examine the type of a variable. For example, the
instanceofoperator in Java allows one to test for types explicitly:

```
/* This is Java code*/
```
```
public void sit( Furniture f ) {
if (f instanceof Chair) {
System.out.println("Sit up straight!");
} else if ( f instanceof Couch ) {
System.out.println("You may slouch.");
} else {
System.out.println("You may sit normally.");
}
}
```
In an **explicitly typed language** , the programmer is responsible for in-
dicating the types of variables and other items in the code explicitly. This
requires more effort on the programmer’s part, but reduces the possibility
of unexpected errors. For example, in an explicitly typed language like C,
the following code might result in an error or warning, due to the loss of
precision when assigning a floating point to an integer:^1

/* This is C code */
int x = 32.5;

Explicit typing can also be used to prevent assignment between vari-
ables that have the same underlying representation, but different mean-
ings. For example, in C and C++, pointers to different types have the same
implementation (a pointer) but it makes no sense to interchange them. The
following should generate an error or at least a warning:

/* This is C code */
int *i;
float *f = i;

In an **implicitly typed language** , the compiler will infer the type of
variables and expressions (to the degree possible) without explicit input
from the programmer. This allows for programs to be more compact, but
can result in accidental behavior. For example, recent C++ standards allow
a variable to be declared with automatic typeauto, like this:

/* This is C++11 code */
auto x = 32.5;
cout << x << endl;

(^1) Not all C compilers will generate a warning, but they should!


##### 7.2. DESIGNING A TYPE SYSTEM 103

The compiler determines that 32.5 has typedouble, and thereforex
must also have typedouble. In a similar way, the output operator<<is
defined to have a certain behavior on integers, another behavior on strings,
and so forth. In this case, the compiler already determined that the type of
xisdoubleand so it chooses the variant of<<that operates on doubles.

### 7.2 Designing a Type System

To describe the type system of a language, we must explain its atomic
types, its compound types, and the rules for assigning and converting be-
tween types.
The **atomic types** of a language are the simple types used to describe
individual variables that are typically (though not always) stored in single
registers in assembly language: integers, floating point numbers, boolean
values, and so forth. For each atomic type, it is necessary to clearly define
the range that is supported. For example, integers may be signed or un-
signed, be 8 or 16 or 32 or 64 bits; floating point numbers could be 32 or 40
or 64 bits; characters could be ASCII or Unicode.
Many languages allow for **user-defined types** in which the program-
mer defines a new type that is implemented using an atomic type, but
gives it a new meaning by restricting the range. For example, in Ada, you
might define new types for days and months:

-- This is Ada code
type Day is range 1..31;
type Month is range 1..12;

This is useful because variables and functions dealing with days and
months are now kept separate, preventing you from accidentally assigning
one to another, or for giving the value 13 to a variable of typeMonth.
C has a similar feature, but it is much weaker:typedefdeclares a new
name for a type, but doesn’t have any means of restricting the range, and
doesn’t prevent you from making assignments between types that share
the same base type:

/* This is C code */
typedef int Month;
typedef int Day;

/* Assigning m to d is allowed in C,
because they are both integers. */

Month m = 10;
Day d = m;


##### 104 CHAPTER 7. SEMANTIC ANALYSIS

**Enumerations** are another kind of user-defined type in which the pro-
grammer indicates a finite set of symbolic values that a variable can con-
tain. For example, if you are working with uncertain boolean variables in
Rust, you might declare:

/* This is Rust code */
enum Fuzzy { True, False, Uncertain };

Internally, an enumeration value is simply an integer, but it makes the
source code more readable, and also allows the compiler to prevent the
programmer from assigning an illegal value. Once again, the C language
allows you to declare enumerations, but doesn’t prevent you from mixing
and matching integers and enumerations.
The **compound types** of a language combine together existing types
into more complex aggregations. You are surely familiar with a **struc-
ture type** (or **record type** ) that groups together several values into a larger
whole. For example, you might group together latitude and longitude to
treat them as a singlecoordinatesstructure:

/* This is Go code */
type coordinates struct {
latitude float64
longitude float64
}

Less frequently used are **union types** in which multiple symbols oc-
cupy the same memory. For example, in C, you can declare a union type
ofnumberthat contains an overlapping float and integer:

/* This is C code */
union number {
int i;
float f;
};

union number n;
n.i = 10;
n.f = 3.14;

In this case,n.iandn.foccupy the same memory. If you assign 10
ton.iand read it back, you will see 10 as expected. However, if you
assign 10 ton.iand read backn.f, you will likely observe a garbage
value, depending on how exactly the two values are mapped into memory.
Union types are occasionally handy when implementing operating system
features such as device drivers, because hardware interfaces often re-use
the same memory locations for multiple purposes.


##### 7.2. DESIGNING A TYPE SYSTEM 105

Some languages provide a **variant type** which allows the programmer
to explicitly describe a type with multiple variants, each with different
fields. This is similar to the concept of a union type, but prevents the
programmer from performing unsafe accesses. For example, Rust allows
us to create a variant type representing an expression tree:

/* This is Rust code */
enum Expression {
ADD{ left: Expression, right: Expression },
MULTIPLY{ left: Expression, right: Expression },
INTEGER{ value: i32 },
NAME{ name: string }
}

This variant type is tightly controlled so that it is difficult to use incor-
rectly. For an Expression of typeADD, it hasleftandrightfields which
can be used in the expected way. For an Expression of typeNAME, the
namefield can be used. The other fields are simply not available unless
the appropriate type is selected.
Finally, we must define what happens when unlike types are used to-
gether. Suppose that an integeriis assigned to a floating pointf. A
similar situation arises when an integer is passed to a function expecting
a floating point as an argument. There are several possibilities for what a
language may do in this case:

- **Disallow the assignment.** A very strict language (like B-Minor)
    could simply emit an error and prevent the program from compil-
    ing! Perhaps it simply makes no sense to make the assignment,
    and the compiler is saving the programmer from a grievous error.
    If the assignment isreallydesired, it could be accomplished by re-
    quiring that the programmer call a built-in conversion function (e.g.
    IntToFloat) that accepts one type and returns another.
- **Perform a bitwise copy.** If the two variables have the same under-
    lying storage size, the unlike assignment could be accomplished by
    just copying the bits in one variable to the location of the other. This
    isusuallya bad idea, since there is no guarantee that one data type
    has any meaning in the other context. But it does happen in a few
    select cases, such as when assigning different pointer types in C.
- **Convert to an equivalent value.** For certain types, the compiler may
    have built-in conversions that change the value to the desired type
    implicitly. For example, it is common to implicitly convert between
    integers and floating points, or between signed and unsigned inte-
    gers. But this does not mean the operation is safe! An implied con-
    version can lose information, resulting in very tricky bugs.


##### 106 CHAPTER 7. SEMANTIC ANALYSIS

- **Interpret the value in a different way.** In some cases, it may be de-
    sirable to convert the value into some other value that is not equiva-
    lent, but still useful for the programmer. For example, in Perl, when
    a list is copied to a scalar context, thelengthof the list is placed in the
    target variable, rather than the content of the list.

```
@days = ("Monday", "Tuesday", "Wednesday", ... );
@a = @days; # copies the array to array a
$b = @days; # puts the length of the array into b
```
### 7.3 The B-Minor Type System

The B-Minor type system is safe, static, and explicit. As a result, it is fairly
compact to describe, straightforward to implement, and eliminates a large
number of programming errors. However, it may be more strict than some
languages, so there will be a large number of errors that we must detect.
B-Minor has the following atomic types:

- integer- A 64 bit signed integer.
- boolean- Limited to symbolstrueorfalse.
- char- Limited to ASCII values.
- string- ASCII values, null terminated.
- void- Only used for a function that returns no value.

And the following compound types:

- array [size] type
- function type ( a: type, b: type, ... )

And here are the type rules that must be enforced:

- A value may only be assigned to a variable of the same type.
- A function parameter may only accept a value of the same type.
- The type of areturnstatement must match the function return
    type.
- All binary operators must have the same type on the left and right
    hand sides.
- The equality operators!=and==may be applied to any type except
    void,array, orfunctionand always returnboolean.


##### 7.4. THE SYMBOL TABLE 107

- The comparison operators < <= >= >may only be applied to
    integervalues and always returnboolean.
- The boolean operators! && ||may only be applied toboolean
    values and always returnboolean.
- The arithmetic operators+ -* / % ˆ ++ --may only be ap-
    plied tointegervalues and always returninteger.

### 7.4 The Symbol Table

The **symbol table** records all of the information that we need to know
about every declared variable (and other named items, like functions) in
the program. Each entry in the table is astruct symbolwhich is shown
in Figure 7.1.

struct symbol {
symbol_t kind;
struct type *type;
char *name;
int which;
};

```
typedef enum {
SYMBOL_LOCAL,
SYMBOL_PARAM,
SYMBOL_GLOBAL
} symbol_t;
```
```
Figure 7.1: The Symbol Structure
```
Thekindfield indicates whether the symbol is a local variable, a global
variable, or a function parameter. Thetypefield points to a type structure
indicating the type of the variable. Thenamefield gives the name (obvi-
ously), and thewhichfield gives the ordinal position of local variables
and parameters. (More on that later.)

As with all the other data structures we have created so far, we must
have a factory function like this:

struct symbol * symbol_create( symbol_t kind,
struct type *type,
char *name ) {
struct symbol *s = malloc(sizeof(*s));
s->kind = kind;
s->type = type;
s->name = name;
return s;
}

To begin semantic analysis, we must create a suitablesymbolstructure
for each variable declaration, and enter it into the symbol table.


##### 108 CHAPTER 7. SEMANTIC ANALYSIS

Conceptually, the symbol table is just a map between the name of each
variable, and the symbol structure that describes it:

```
Variable
Names
```
```
Symbol
Table
```
```
Symbol
Structures
```
However, it’s notquitethat simple, because most programming lan-
guages allow the same variable name to be used multiple times, as long
as each definition is in a distinct **scope**. In C-like languages (including B-
Minor) there is a global scope, a scope for function parameters and local
variables, and then nested scopes everywhere curly braces appear.
For example, the following B-Minor program defines the symbolx
three times, each with a different type and storage class. When run, the
program should print10 hello false.

x: integer = 10;

f: function void ( x: string ) =
{
print x, "\n";
{
x: boolean = false;
print x, "\n";
}
}

main: function void () =
{
print x, "\n";
f("hello");
}


##### 7.4. THE SYMBOL TABLE 109

To accommodate these multiple definitions, we will structure our sym-
bol table as a stack of hash tables, as shown in Figure 7.2. Each hash table
maps the names in a given scope to their corresponding symbols. This
allows a symbol (likex) to exist in multiple scopes without conflict. As
we proceed through the program, we will push a new table every time a
scope is entered, and pop a table every time a scope is left.

```
x
```
```
Global
Scope
Table
```
```
x
```
```
Function
Scope
Table
```
```
x
```
```
Inner
Scope
Table
```
```
f
```
```
main
```
```
Stack Top
```
```
symbol
x LOCAL(0) BOOLEAN
```
```
symbol
x PARAM(0) STRING
```
```
symbol
x GLOBAL INTEGER
```
```
symbol
f GLOBAL FUNCTION
```
```
symbol
mainGLOBALFUNCTION
```
```
Figure 7.2: A Nested Symbol Table
```

##### 110 CHAPTER 7. SEMANTIC ANALYSIS

void scope_enter();
void scope_exit();
int scope_level();

void scope_bind( const char *name, struct symbol *sym );
struct symbol *scope_lookup( const char *name );
struct symbol *scope_lookup_current( const char *name );

```
Figure 7.3: Symbol Table API
```
To manipulate the symbol table, we define six operations in the API
given in Figure 7.3. They have the following meanings:

- scopeenter()causes a new hash table to be pushed on the top of
    the stack, representing a new scope.
- scopeexit()causes the topmost hash table to be removed.
- scopelevel()returns the number of hash tables in the current
    stack. (This is helpful to know whether we are at the global scope or
    not.)
- scopebind(name,sym)adds an entry to the topmost hash table
    of the stack, mappingnameto the symbol structuresym.
- scopelookup(name)searches the stack of hash tables from top to
    bottom, looking for the first entry that matchesnameexactly. If no
    match is found, it returns null.
- scopelookupcurrent(name)works likescopelookupexcept
    that it only searches the topmost table. This is used to determine
    whether a symbol has already been defined in the current scope.


##### 7.5. NAME RESOLUTION 111

### 7.5 Name Resolution

With the symbol table in place, we are now ready to match each use of a
variable name to its matching definition. This process is known as **name
resolution**. To implement name resolution, we will write aresolve
method for each of the structures in the AST, includingdeclresolve(),
stmtresolve()and so forth.
Collectively, these methods must iterate over the entire AST, looking
for variable declarations and uses. Wherever a variable is declared, it must
be entered into the symbol table and thesymbolstructure linked into the
AST. Wherever a variable is used, it must be looked up in the symbol table,
and thesymbolstructure linked into the AST. Of course, if a symbol is
declared twice in the same scope, or used without declaration, then an
appropriate error message must be emitted.
We will begin with declarations, as shown in Figure 7.4. Eachdecl
represents a variable declaration of some kind, sodeclresolvewill cre-
ate a new symbol, and then bind it to the name of the declaration in the
current scope. If the declaration represents an expression (d->valueis
not null) then the expression should be resolved. If the declaration repre-
sents a function (d->codeis not null) then we must create a new scope
and resolve the parameters and the code.
Figure 7.4 gives some sample code for resolving declarations. As al-
ways in this book, consider this starter code in order to give you the basic
idea. You will have to make some changes in order to accommodate all
the features of the language, handle errors cleanly, and so forth.
In a similar fashion, we must write resolve methods for each structure
in the AST.stmtresolve()(not shown) must simply call the appropri-
ateresolveon each of its sub-components. In the case of a STMTBLOCK,
it must also enter and leave a new scope.paramlistresolve()(also
not shown) must enter a new variable declaration for each parameter of a
function, so that those definitions are available to the code of a function.
To perform name resolution on the entire AST, you may simply invoke
declresolve()once on the root node of the AST. This function will
traverse the entire tree by calling the necessary sub-functions.


##### 112 CHAPTER 7. SEMANTIC ANALYSIS

void decl_resolve( struct decl *d )
{
if(!d) return;

```
symbol_t kind = scope_level() > 1?
SYMBOL_LOCAL : SYMBOL_GLOBAL;
```
```
d->symbol = symbol_create(kind,d->type,d->name);
```
```
expr_resolve(d->value);
scope_bind(d->name,d->symbol);
```
```
if(d->code) {
scope_enter();
param_list_resolve(d->type->params);
stmt_resolve(d->code);
scope_exit();
}
```
decl_resolve(d->next);
}

```
Figure 7.4: Name Resolution for Declarations
```
void expr_resolve( struct expr *e )
{
if(!e) return;

if( e->kind==EXPR_NAME ) {
e->symbol = scope_lookup(e->name);
} else {
expr_resolve( e->left );
expr_resolve( e->right );
}
}

```
Figure 7.5: Name Resolution for Expressions
```

##### 7.6. IMPLEMENTING TYPE CHECKING 113

### 7.6 Implementing Type Checking

Before checking expressions, we need some helper functions for check-
ing and manipulating type structures. Here is pseudo-code for checking
equality, copying, and deleting types:

boolean type_equals( struct type *a, struct type *b )
{
if( a->kind == b->kind ) {
if( a and b are atomic types ){
Return true
} else if ( both are array ) {
Return true if subtype is recursively equal
} else if ( both are function ) {
Return true if both subtype and params
are recursively equal
}
} else {
Return false
}
}

struct type * type_copy( struct type*t )
{
Return a duplicate copy of t, making sure
to duplicate subtype and params recursively.
}

void type_delete( struct type *t )
{
Free all the elements of t recursively.
}

Next, we construct a functionexprtypecheckthat will compute the
proper type of an expression, and return it. To simplify our code, we assert
thatexprtypecheck, if called on a non-nullexpr, will always return
a newly-allocatedtypestructure. If the expression contains an invalid
combination of types, thenexprtypecheckwill print out an error, but
return a valid type, so that the compiler can continue on and find as many
errors as possible.
The general approach is to perform a recursive, post-order traversal of
the expression tree. At the leaves of the tree, the type of the node simply
corresponds to the kind of the expression node: an integer literal has inte-
ger type, a string literal has string type, and so on. If we encounter a vari-
able name, the type can be determined by following thesymbolpointer


##### 114 CHAPTER 7. SEMANTIC ANALYSIS

to the symbol structure, which contains the type. This type is copied and
returned to the parent node.
For interior nodes of the expression tree, we must compare the type
of the left and right subtrees, and determine if they are compatible with
the rules indicated in Section 7.3. If not, we emit an error message and
increment a global error counter. Either way, we return the appropriate
type for the operator. The types of the left and right branches are no longer
needed and can be deleted before returning.
Here is the basic code structure:

struct type * expr_typecheck( struct expr *e )
{
if(!e) return 0;

```
struct type *lt = expr_typecheck(e->left);
struct type *rt = expr_typecheck(e->right);
```
```
struct type *result;
```
```
switch(e->kind) {
case EXPR_INTEGER_LITERAL:
result = type_create(TYPE_INTEGER,0,0);
break;
case EXPR_STRING_LITERAL:
result = type_create(TYPE_STRING,0,0);
break;
```
```
/* more cases here*/
```
##### }

```
type_delete(lt);
type_delete(rt);
```
return result;
}


##### 7.6. IMPLEMENTING TYPE CHECKING 115

Let’s consider the cases for a few operators in detail. Arithmetic oper-
ators can only be applied to integers, and always return an integer type:

case EXPR_ADD:
if( lt->kind!=TYPE_INTEGER ||
rt->kind!=TYPE_INTEGER ) {
/* display an error*/
}
result = type_create(TYPE_INTEGER,0,0);
break;

The equality operators can be applied to most types, as long as the
types are equal on both sides. These always return boolean.

case EXPR_EQ:
case EXPR_NE:
if(!type_equals(lt,rt)) {
/* display an error*/
}
if(lt->kind==TYPE_VOID ||
lt->kind==TYPE_ARRAY ||
lt->kind==TYPE_FUNCTION) {
/* display an error*/
}
result = type_create(TYPE_BOOLEAN,0,0);
break;

An array dereference likea[i]requires thatabe an array,ibe an
integer, and returns the subtype of the array:

case EXPR_DEREF:
if(lt->kind==TYPE_ARRAY) {
if(rt->kind!=TYPE_INTEGER) {
/* error: index not an integer */
}
result = type_copy(lt->subtype);
} else {
/* error: not an array*/
/* but we need to return a valid type */
result = type_copy(lt);
}
break;

Most of the hard work in typechecking is done inexprtypecheck,
but we still need to implement typechecking on declarations, statements,
and the other elements of the AST.decltypecheck,stmttypecheck


##### 116 CHAPTER 7. SEMANTIC ANALYSIS

and the other typechecking methods simply traverse the AST, compute the
type of expressions, and then check them against declarations and other
constraints as needed.
For example,decltypechecksimply confirms that variable declara-
tions match their initializers and otherwise typechecks the body of func-
tion declarations:

void decl_typecheck( struct decl *d )
{
if(d->value) {
struct type *t;
t = expr_typecheck(d->value);
if(!type_equals(t,d->symbol->type)) {
/* display an error*/
}
}
if(d->code) {
stmt_typecheck(d->code);
}
}

Statements must be typechecked by evaluating each of their compo-
nents, and then verifying that types match where needed. After the type
is examined, it is no longer needed and may be deleted. For example,
if-then statements require that the control expression have boolean type:

void stmt_typecheck( struct stmt *s )
{
struct type *t;
switch(s->kind) {
case STMT_EXPR:
t = expr_typecheck(s->expr);
type_delete(t);
break;
case STMT_IF_THEN:
t = expr_typecheck(s->expr);
if(t->kind!=TYPE_BOOLEAN) {
/* display an error*/
}
type_delete(t);
stmt_typecheck(s->body);
stmt_typecheck(s->else_body);
break;
/* more cases here*/
}
}


##### 7.7. ERROR MESSAGES 117

### 7.7 Error Messages

Compilers in general are notorious for displaying terrible error messages.
Fortunately, we have developed enough code structure that it is straight-
forward to display an informative error message that explains exactly what
types were discovered, and what the problem is.
For example, this bit of B-Minor code has a mess of type problems:

s: string = "hello";
b: boolean = false;
i: integer = s + (b<5);

```
Most compilers would emit an unhelpful message like this:
```
error: type compatibility in expression

But, your project compiler can very easily have much more detailed
error messages like this:

error: cannot compare a boolean (b) to an integer (5)
error: cannot add a boolean (b<5) to a string (s)

It’s just a matter of taking some care in printing out each of the expres-
sions and types involved when a problem is found:

printf("error: cannot add a ");
type_print(lt);
printf(" (");
expr_print(e->left);
printf(") to a ");
type_print(rt);
printf(" (");
expr_print(e->right);
printf(")\n");


##### 118 CHAPTER 7. SEMANTIC ANALYSIS

**7.8 Exercises**

1. Implement the symbol and scope functions in symbol.c and
    scope.c, using an existing hash table implementation as a starting
    point.
2. Complete the name resolution code by writingstmtresolve()
    andparamlistresolve()and any other supporting code needed.
3. Modifydeclresolve()andexprresolve()to display errors
    when the same name is declared twice, or when a variables is used
    without a declaration.
4. Complete the implementation ofexprtypecheckso that it checks
    and returns the type of all kinds of expressions.
5. Complete the implementation ofstmttypecheckby enforcing the
    constraints particularly to each kind of statement.
6. Write a functionmyprintfthat displays printf-style format strings,
    but supports symbols like%Tfor types,%Efor expressions, and so
    forth. This will make it easier to emit error messages, like this:

```
myprintf(
"error: cannot add a %T (%E) to a %T (%E)\n",
lt,e->left,rt,e->right
);
```
```
Consult a standard C manual and learn about the functions in the
stdarg.hheader for creating variadic functions.
```
### 7.9 Further Reading

1. H. Abelson, G. Sussman, and J. Sussman, “Structure and Interpreta-
    tion of Computer Programs”, MIT Press, 1996.
2. B. Pierce, “Types and Programming Languages”, MIT Press, 2002.
3. D. Friedman and D. Christiansen, “The Little Typer”, MIT Press,
    2018.


##### 119

## Chapter 8 – Intermediate Representations

### 8.1 Introduction

Most production compilers make use of an **intermediate representation
(IR)** that lies somewhere between the abstract structure of the source lan-
guage and the concrete structure of the target assembly language.
An IR is designed to have a simple and regular structure that facilitates
optimization, analysis, and efficient code generation. A modular compiler
will often implement each optimization or analysis tool as a separate mod-
ule that consumes and produces the same IR, so that it is easy to select and
compose optimizations in different orders.
It is common for an IR to have a defined **external format** that can be
written out to a file in text form, so that it can be exchanged between unre-
lated tools. Although it may be visible to the determined programmer, it
usually isn’t meant to be easily readable. When loaded into memory, the
IR is represented as a data structure, to facilitate algorithms that traverse
its structure.
There are many different kinds of IR that can be used; some are very
close to the AST we used up to this point, while others are only a very
short distance from the target assembly language. Some compilers even
use multiple IRs in decreasing layers of abstraction. In this chapter, we
will examine different approaches to IRs and consider their strengths and
weaknesses.

### 8.2 Abstract Syntax Tree

First, we will point out that the AST itself can be a usable IR, if the goal is
simply to emit assembly language without a great deal of optimization or
other transformations. Once typechecking is complete, simple optimiza-
tions like strength reduction and constant folding can be applied to the
AST itself. Then, to generate assembly language, you can simply perform
a post-order traversal of the AST and emit a few assembly instructions
corresponding to each node.^1

(^1) This is the approach we use in a one-semester course to implement a project compiler,
since there is a limited amount of time to get to the final goal of generating assembly lan-
guage.


##### 120 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

typedef enum {
DAG_ASSIGN,
DAG_DEREF,
DAG_IADD,
DAG_IMUL,

DAG_NAME,
DAG_FLOAT_VALUE,
DAG_INTEGER_VALUE
} dag_kind_t;

struct dag_node {
dag_kind_t kind;
struct dag_node *left;
struct dag_node *right;
union {
const char *name;
double float_value;
int integer_value;
} u;
};

```
Figure 8.1: Sample DAG Data Structure Definition
```
However, in a production compiler, the AST isn’t a great choice for an
IR, mainly because the structure istoorich. Each node has a large number
of different options and substructure: for example, an addition node could
represent an integer addition, a floating point addition, a boolean-or, or a
string concatenation, depending on the types of the values involved. This
makes it difficult to perform robust transformations, as well as to generate
an external representation. A more low-level representation is needed.

### 8.3 Directed Acyclic Graph

The **directed acyclic graph (DAG)** is one step simplified from the AST. A
DAG is similar to the AST, except that it can have an arbitrary graph struc-
ture, and the individual nodes are greatly simplified, so that there is little
or no auxiliary information beyond the type and value of each node. This
requires that we have a greater number of node types, each one explicit
about its purpose. For example, Figure 8.1 shows a definition of a DAG
data structure that would be compatible with our project compiler.


##### 8.3. DIRECTED ACYCLIC GRAPH 121

Now suppose we compile a simple expression likex=(a+10)*(a+10).
The AST representation of this expression would directly capture the syn-
tactic structure:

```
ADD
```
```
a 10
```
```
ASSIGN
```
```
x MUL
```
```
ADD
```
```
a 10
```
After performing typechecking, we may learn thatais a floating point
value, and therefore 10 must be converted into a float before performing
floating point arithmetic. In addition, the computationa+10need only be
performed once, and the resulting value used twice.
All of that can be represented with the following DAG, which intro-
duces a new type of nodeITOFto perform integer-to-float conversion,
and nodesFADDandFMULto perform floating point arithmetic:

```
ASSIGN
```
```
x FMUL
```
```
FADD
```
```
a ITOF
```
```
10
```

##### 122 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

It is also common for a DAG to represent address computations related
to pointers and arrays in greater detail, so that they can be shared and
optimized, where possible. For example, the array lookupx=a[i]would
have a very simple representation in the AST:

```
ASSIGN
```
```
x LOOKUP
```
```
a i
```
However, an array lookup is actually accomplished by adding the start-
ing address of the arrayawith the index of the itemimultiplied by the
size of objects in the array, determined by consulting the symbol table.
This could be expressed in a DAG like this:

```
ASSIGN
```
```
x DEREF
```
```
IADD
```
```
a IMUL
```
```
i 4
```
As a final step before code generation, the DAG might be expanded
to include the address computations for local variables. For example, ifa
andiare stored on the stack at sixteen and twenty bytes past the frame
pointerFP, respectively, the DAG could be expanded like this:


##### 8.3. DIRECTED ACYCLIC GRAPH 123

```
DEREF
```
```
IADD
```
```
FP 16
```
```
DEREF
```
```
IADD
```
```
FP 20
```
```
ASSIGN
```
```
x DEREF
```
```
IADD
```
```
IMUL
```
```
4
```
The **value-number method** can be used to construct a DAG from an
AST. The idea is to build an array where each entry consists of a DAG
node type, and the array index of the child nodes. Every time we wish to
add a new node to the DAG, we search the array for a matching node and
re-use it to avoid duplication. The DAG is constructed by performing a
post-order traversal of the AST and adding each element to the array.
The DAG above could be represented by this value-number array:

```
# Type Left Right Value
0 NAME x
1 NAME a
2 INT 10
3 ITOF 2
4 FADD 1 3
5 FMUL 4 4
6 ASSIGN 0 5
```
Obviously, searching the table for equivalent nodes every time a new
node gets added is going to have polynomial complexity. However, the
absolute sizes stay relatively small, as long as each individual expression
has its own DAG.


##### 124 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

By designing the DAG representation such that all necessary informa-
tion is encoded into the node type, it becomes easy to write a portable
external representation. For example, we could represent each node as a
symbol, followed by its children in parentheses:

ASSIGN(x,DEREF(IADD(DEREF(IADD(FP,16)),
IMUL(DEREF(IADD(FP,20)),4))))

Clearly, this sort of code would not be easy for a human to read and
write manually, but it is trivial to print and trivial to parse, making it easy
to pass between compiler stages for analysis and optimization.
Now, what sort of optimizations might you do with the DAG? One
easy optimization is **constant folding**. This is the process of reducing an
expression consisting of only constants into a single value.^2 This capabil-
ity is handy, because the programmer may wish to leave some expressions
in an explicit form for the sake of readability or maintainability while still
having the performance advantage of a single constant in the executable
code.

```
DAG Constant Folding Algorithm
Examine a DAG recursively and collapse all operators on
two constants into a single constant.
```
```
ConstantFold( DagNode n ):
```
```
If n is a leaf:
return;
Else:
n.left = ConstantFold(n.left);
n.right = ConstantFold(n.right);
```
```
If n.left and n.right are constants:
n.value = n.operator(n.left,n.right);
n.kind = constant;
delete n.left and n.right;
```
(^2) Constant folding is a narrow example of the more general technique of **partial execution**
in which some parts of the program are executed at compile time, while the rest is left for
runtime.


##### 8.4. CONTROL FLOW GRAPH 125

```
Figure 8.2: Example of Constant Folding
```
```
IMUL
```
```
24 IMUL
```
```
60 60
```
```
ASSIGN
```
```
secs IMUL
```
```
days
```
```
IMUL
```
```
24 3600
```
```
ASSIGN
```
```
secs IMUL
```
```
days
```
```
ASSIGN
```
```
secs IMUL
```
```
days 86400
```
Suppose you have an expression that computes the number of sec-
onds present in the number of days. The programmer expresses this as

secs=days* (^24) * (^60) * 60 to make it clear that there are 24 hours in a day,
60 minutes in an hour, and 60 seconds in a minute. Figure 8.2 shows how
ConstantFoldwould reduce the DAG. The algorithm descends through
the tree and combinesIMUL(60,60)into 3600 , and thenIMUL(3600,24)
into 86400.

### 8.4 Control Flow Graph

It is important to note that a DAG by itself is suitable for encoding expres-
sions, but it isn’t as effective for control flow or other ordered program
structures. Common sub-expressions are combined under the assumption
that they can be evaluated in any order (consistent with operator prece-
dence) and the values already in the DAG do not change. This assumption
does not hold when we consider multiple statements that modify values,
or control flow structures that repeat or skip statements.
To reflect this, we can use a **control flow graph** to represent the higher-
level structure of the program. The control flow graph is a directed graph
(possibly cyclic) where each node of the graph consists of a **basic block**
of sequential statements. The edges of the graph represent the possible
flows of control between basic blocks. A conditional construct (likeifor
switch) results in branches in the graph, while a loop construct (likefor
orwhile) results in reverse edges.


##### 126 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

```
For example, this bit of code:
```
for(i=0;i<10;i++) {
if(i%2==0) {
print "even";
} else {
print "odd";
}
print "\n";
}
return;

```
Would result in this control flow graph:
```
```
i=0;
```
```
if(i<10)
```
```
if(i%2==0)
```
```
true
```
```
return;
```
```
false
```
```
print "even";
```
```
true
```
```
print "odd";
```
```
false
```
```
print "\n";
```
```
i++;
```
```
Figure 8.3: Example Control Flow Graph
```
Note that the control flow graph has a different structure than the AST.
The AST for a for-loop would have each of the control expressions as an
immediate child of the loop node, whereas the control flow graph places
each one in the order it would be executed in practice. Likewise, theif
statement has edges from each branch of the conditional to the following
node, so that one can easily trace the flow of execution from one compo-
nent to the next.


##### 8.5. STATIC SINGLE ASSIGNMENT FORM 127

### 8.5 Static Single Assignment Form

The **static single assignment (SSA)** [1]form is a commonly-used represen-
tation for complex optimizations. SSA uses the information in the control
flow and updates each basic block with a new restriction:variables cannot
change their values. Instead, whenever a variable is assigned a new value,
it is given a new version number.
For example, suppose that we have this bit of code:

int x = 1;
int a = x;
int b = a + 10;
x = 20 * b;
x = x + 30;

```
We could re-write it in SSA form like this:
```
int x_1 = 1;
int a_1 = x_1;
int b_1 = a_1 + 10;
x_2 = 20 * b_1;
x_3 = x_2 + 30;

A peculiarity comes when a variable is given a different value in two
branches of a conditional. Following the conditional, the variable could
have either value, but we don’t know which one. To express this, we in-
troduce a new functionφ(x,y)which indicates that either valuexory
could be selected at runtime. Theφfunction may not necessarily translate
to an instruction in the assembly output, but serves to link the new value
to its possible old values, reflecting the control flow graph.
For example, this code fragment:

if(y<10) {
x=a;
} else {
x=b;
}

```
Becomes this:
```
if(y_1<10) {
x_2=a;
} else {
x_3=b;
}
x_4 = phi(x_2,x_3);


##### 128 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

### 8.6 Linear IR

A linear IR is an ordered sequence of instructions that is closer to the final
goal of an assembly language. It loses some of the flexibility of a DAG
(which does not commit to a specific ordering) but can capture expres-
sions, statements, and control flow all within one data structure. This en-
ables some optimization techniques that span multiple expressions.
There is no universal standard for a linear IR. A linear IR typically looks
like an idealized assembly language with a large (or infinite) number of
registers and the usual arithmetic and control flow operations. Here, let
us assume an IR whereLOADandSTORare used to move values between
memory and registers, and three-address arithmetic operations read two
registers and write to a third, from left to right. Our example expression
would look like this:

1. LOAD a -> %r1
2. LOAD $10 -> %r2
3. ITOF %r2 -> %r3
4. FADD %r1, %r3 -> %r4
5. FMUL %r4, %r4 -> %r5
6. STOR %r5 -> x

This IR is easy to store efficiently, because each instruction can be a
fixed size 4-tuple representing the operation and (max) of three arguments.
The external representation is also straightforward.
As the example suggests, it is most convenient to pretend that there are
an infinite number of **virtual registers** , such that every new value com-
puted writes to a new register. In this form, we can easily identify the
**lifetime** of a value by observing the first point where a register is written,
and the last point where a register is used. Between those two points, the
value of register one must be preserved. For example, the lifetime of%r1
is from instruction 1 to instruction 4.
At any given instruction, we can also observe the set of virtual registers
that are live:

1. LOAD a -> %r1 live: %r1
2. LOAD $10 -> %r2 live: %r1 %r2
3. ITOF %r2 -> %r3 live: %r1 %r2 %r3
4. FADD %r1, %r3 -> %r4 live: %r1 %r3 %r4
5. FMUL %r4, %r4 -> %r5 live: %r4 %r5
6. STOR %r5 -> x live: %r5

This observation makes it easy to perform operations related to instruc-
tion ordering. Any instruction may be moved to an earlier position (within
one basic block) as long as the values it reads are not moved above their
definitions. Likewise, any instruction may be moved to a later position


##### 8.7. STACK MACHINE IR 129

as long as the values it writes are not moved below their uses. Moving
instructions can reduce the number of physical registers needed in code
generation, as well as reduce execution time in a pipelined architecture.

### 8.7 Stack Machine IR

An even more compact intermediate representation is a **stack machine** IR.
Such a representation is designed to execute on a **virtual stack machine**
that has no traditional registers, but only a stack to hold intermediate reg-
isters. A stack machine IR typically has aPUSHinstruction which pushes
a variable or literal value on to the stack and aPOPinstruction which re-
moves an item and stores it in memory. Binary arithmetic operators (like
FADDorFMUL) implicitly pop two values off the stack and push the result
on the stack, while unary operators (ITOF) pop one value and push one
value. A few utility instructions are needed to manipulate the stack, like a
COPYinstruction which pushes a duplicate value on to the stack.
To emit a stack machine IR from a DAG, we simply perform a post-
order traversal of the AST and emit aPUSHfor each leaf value, an arith-
metic instruction for each interior node, and aPOPinstruction to assign a
value to a variable.
Our example expression would look like this in a stack machine IR:

PUSH a
PUSH 10
ITOF
FADD
COPY
FMUL
POP x

If we suppose thatahas the value 5.0, then executing the IR directly
would result in this:

IR Op: PUSH a PUSH 10 ITOF FADD COPY FMUL POP x
Stack 5.0 10 10.0 15.0 15.0 225.0 -
State: - 5.0 5.0 - 15.0 - -
A stack machine IR has many advantages. It is much more compact
than a 3-tuple or 4-tuple linear representation, since there is no need to
record the details of registers. It is also straightforward to implement this
language in a simple interpreter.
However, a stack-based IR is slightly more difficult to translate to a
conventional register-based assembly language, precisely because the ex-
plicit register names are lost. Further transformation or optimization of
this form requires that we transform the implicit information dependen-
cies in the stack-basic IR back into a more explicit form such as a DAG or
a linear IR with explicit register names.


##### 130 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

### 8.8 Examples

Nearly every compiler or language has its own intermediate representa-
tion with some peculiar local features. To give you a sense of what’s pos-
sible, this section compares three different IRs used by compilers in 2017.
For each one, we will show the output of compiling this simple arithmetic
expression:

float f( int a, int b, float x ) {
float y = a*x*x + b*x + 100;
return y;
}

#### 8.8.1 GIMPLE - GNU Simple Representation

The **GNU Simple Representation (GIMPLE)** is an internal IR used at the
earliest stages of the GNU C compiler. GIMPLE can be thought of as a
drastically simplified form of C in which all expressions have been broken
down into individual operators on values in static single assignment form.
Basic conditionals are allowed, and loops are implemented usinggoto.
Our simple function yields the following GIMPLE. Note that each SSA
value is declared as a local variable (with a long name) and the type of
each operator is still inferred from the local type declaration.

f (int a, int b, float x)
{
float D.1597D.1597;
float D.1598D.1598;
float D.1599D.1599;
float D.1600D.1600;
float D.1601D.1601;
float D.1602D.1602;
float D.1603D.1603;
float y;

D.1597D.1597 = (float) a;
D.1598D.1598 = D.1597D.1597 * x;
D.1599D.1599 = D.1598D.1598 * x;
D.1600D.1600 = (float) b;
D.1601D.1601 = D.1600D.1600 * x;
D.1602D.1602 = D.1599D.1599 + D.1601D.1601;
y = D.1602D.1602 + 1.0e+2;
D.1603D.1603 = y;
return D.1603D.1603;
}


##### 8.8. EXAMPLES 131

#### 8.8.2 LLVM - Low Level Virtual Machine

The Low Level Virtual Machine (LLVM)^3 project is a language and a corre-
sponding suite of tools for building optimizing compilers and interpreters.
A variety of compiler front-ends support the generation of LLVM interme-
diate code, which can be optimized by a variety of independent tools, and
then translated again into native machine code, or bytecode for virtual
machines like Oracle’s JVM, or Microsoft’s CLR.
Our simple function yields this LLVM. Note that the first fewalloca
instructions allocate space for local variables, followed bystoreinstruc-
tions that move the parameters to local variables. Then, each step of the
expression is computed in SSA form and the result stored to the local vari-
abley. The code is explicit at each step about the type (32-bit integer or
float) and the alignment of each value.

define float @f(i32 %a, i32 %b, float %x) #0 {
%1 = alloca i32, align 4
%2 = alloca i32, align 4
%3 = alloca float, align 4
%y = alloca float, align 4
store i32 %a, i32* %1, align 4
store i32 %b, i32* %2, align 4
store float %x, float* %3, align 4
%4 = load i32* %1, align 4
%5 = sitofp i32 %4 to float
%6 = load float* %3, align 4
%7 = fmul float %5, %6
%8 = load float* %3, align 4
%9 = fmul float %7, %8
%10 = load i32* %2, align 4
%11 = sitofp i32 %10 to float
%12 = load float* %3, align 4
%13 = fmul float %11, %12
%14 = fadd float %9, %13
%15 = fadd float %14, 1.000000e+02
store float %15, float* %y, align 4
%16 = load float* %y, align 4
ret float %16
}

(^3) [http://llvm.org](http://llvm.org)


##### 132 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

#### 8.8.3 JVM - Java Virtual Machine

The **Java Virtual Machine (JVM)** is an abstract definition of a stack-based
machine. High-level code written in Java is compiled into.classfiles
which contain a binary representation of the JVM bytecode. The earliest
implementations of the JVM were interpreters which read and executed
the JVM bytecode in the obvious way. Later implementations performed
just-in-time (JIT) compiling of the bytecode into native assembly language,
which can be executed directly.
Our simple function yields the following JVM bytecode. Note that each
of theiloadinstructions refers to a local variable, where parameters are
considered as the first few local variables. So,iload 1pushes the first
local variable (int a) on to the stack, whilefload 3pushes the third
local variable (float x) on to the stack. Fixed constants are stored in
an array in the class file and referenced by position, soldc #2pushes
constant in position two ( 100 ) on to the stack.

0: iload 1
1: i2f
2: fload 3
4: fmul
5: fload 3
7: fmul
8: iload 2
9: i2f
10: fload 3
12: fmul
13: fadd
14: ldc #2
16: fadd
17: fstore 4
19: fload 4
21: freturn


##### 8.9. EXERCISES 133

### 8.9 Exercises

1. Add a step to your compiler to convert the AST into a DAG by per-
    forming a post-order traversal and creating one or more DAG nodes
    corresponding to each AST node.
2. Write the code to export a DAG in a simple external representation as
    shown in this chapter. Extend the DAG suitably to represent control
    flow structures and function definitions.
3. Write a scanner and parser to read in the DAG external representa-
    tion and reconstruct it as a data structure. Think carefully about the
    grammar class of the IR, and choose the simplest implementation
    that works.
4. Building on steps 2 and 3, write a standalone optimization tool that
    reads in the DAG format, performs a simple optimization like con-
    stant folding, and writes the DAG back out in the same format.


##### 134 CHAPTER 8. INTERMEDIATE REPRESENTATIONS

### 8.10 Further Reading

1. R. Cytron, J. Ferrante, B. Rosen, M. Wegman, and F. Kenneth Zadeck.
    “Efficiently computing static single assignment form and the con-
    trol dependence graph.” ACM Transactions on Programming Lan-
    guages and Systems (TOPLAS) volume 13, number 4, 1991.
    https://doi.org/10.1145/115372.115320
2. J. Merrill, “Generic and GIMPLE: A new tree representation for en-
    tire functions.” GCC Developers Summit, 2003.
3. C. Lattner and V. Adve, “LLVM: A Compilation Framework for Life-
    long Program Analysis & Transformation”, IEEE International Sym-
    posium on Code Generation and Optimization, 2004.
    https://dl.acm.org/citation.cfm?id=977673


##### 135

## Chapter 9 – Memory Organization

### 9.1 Introduction

Before digging into the translation of intermediate code to assembly lan-
guage, we must discuss how the internal memory of a running program
is laid out. Although a process is free to use memory in any way that it
likes, a convention has developed that divides the areas of a program into
logical segments, each with a different internal management strategy.

### 9.2 Logical Segmentation

A conventional program sees memory as a linear sequence of words, each
with a numeric address starting at zero, and increasing up to some large
number (e.g. 4GB on a 32-bit processor.)

```
Program Memory
```
```
0 4GB
```
```
Figure 9.1: Flat Memory Model
```
In principle, the CPU is free to use memory in any way it sees fit. Code
and data could be scattered and intermixed across memory in any order
that is convenient. It is even technically possible for a CPU to modify the
memory containing its code while it is running. It goes without saying that
programming in this fashion would be complex, confusing, and difficult
to debug.

Instead, program memory is commonly laid out by separating it into
**logical segments**. Each segment is a sequential address range, dedicated
to a particular purpose within the program. The segments are typically
laid out in this order:


##### 136 CHAPTER 9. MEMORY ORGANIZATION

```
Code Data Heap→... ←Stack
```
```
0 4GB
```
```
Figure 9.2: Logical Segments
```
- The **code segment** (also known as the **text segment** ) contains the ma-
    chine code of the program, corresponding to the bodies of functions
    in a C program.
- The **data segment** contains the global data of the program, corre-
    sponding to the global variables in a C program. The data seg-
    ment may further be sub-divided into read-write data (variables)
    and read-only data (constants).
- The **heap segment** contains the heap, which is the area of memory
    that is managed dynamically at runtime bymallocandfreein a
    C program, ornewanddeletein other languages. The top of the
    heap is historically known as the **break**.
- The **stack segment** contains the stack, which records the current ex-
    ecution state of the program as well as the local variables currently
    in use.

Typically, the heap grows “up” from lower addresses to higher ad-
dresses, while the stack grows “down” from higher to lower. In between
the two segments is an invalid region of memory that is unused until over-
taken by one segment or the other.
On a simple computer such as an embedded machine or microcon-
troller, logical segments are nothing more than an organizational conven-
tion: nothing stops the program from using memory improperly. If the
heap grows too large, it can run into the stack segment (or vice versa), the
program will crash (if you are lucky) or suffer silent data corruption (if
you are unlucky).
On a computer with an operating system that employs multiprogram-
ming and memory protection, the situation is better. Each process running
in the OS has its own private memory space, with the illusion of starting
at address zero and extending to a high address. As a result, each process
can access its own memory arbitrarily, but is prevented from accessing or
modifying other processes. Within its own space, each process lays out its
own code, data, heap, and stack segments.
In some operating systems, when a program is initially loaded into
memory, permissions are set on each range of memory corresponding to
its purpose: memory corresponding to each segment can be set appropri-


##### 9.2. LOGICAL SEGMENTATION 137

```
OS Kernel Process 1 Process 2
```
```
Code Data Code Data Heap Stk Code Data Heap Stk
Virtual Addrs: 0 −→ 4GB 0 −→ 4GB
```
```
Figure 9.3: Multiprogrammed Memory Layout
```
ately: read-write for data, heap, and stack; read-only for constants; read-
execute for code; and none for the unused area.
The permissions on the logical segments also protect a process from
damaging itself in certain ways. For example, code cannot be modified
at runtime because it is marked read/execute, while items on the heap
cannot be executed, because they are marked read/write. (To be clear, this
only prevents against accidents, not malice; a program can always ask the
operating system to change the permissions on one of its own pages. For
an example, look up themprotectsystem call in Unix.)
If a process attempts to access memory in a prohibited way or attempts
to access the unused area, a **page fault** occurs. This forces a transfer of
control to the OS, which considers the process and the faulting address. If
the access indicates a violation of the logical segmentation of the program,
then the process is forcibly killed with the error message **segmentation
fault**.^1
Initially, a process is given a small amount of memory for the heap
segment, which it manages internally to implementmallocandfree. If
this area is exhausted and the program still needs more, it must explicitly
request it from the operating system. On traditional Unix systems, this
is done with thebrksystem call, which requests that the heap segment
be extended to a new address. If the OS agrees, then it will allocate new
pages at the beginning of the invalid area, effectively extending the heap
segment. If the OS does not agree, thenbrkwill return an error code,
causingmallocto return an error (indicated as a null pointer) which the
program must handle.
The stack has a similar problem, in that it must be able to grow down.
It is not so easy for a program to determine exactly when more stack is
needed, because that happens whenever a new function is invoked, or new
local variables are allocated. Instead, modern operating systems maintain
a **guard page** at the top of the invalid area, adjacent to the current stack.
When a process attempts to extend the stack into the invalid area, a page
fault occurs, and control is transferred to the OS. If the OS sees that the
faulting address is in the guard page, it can simply allocate more pages
for the stack, set the page permissions appropriately, and move the guard
page down to the new top of the invalid area.
Of course, there are limits on how big the heap and the stack may grow;

(^1) And now you understand this mysterious Unix term!


##### 138 CHAPTER 9. MEMORY ORGANIZATION

every OS implements policies controlling how much memory any process
or user may consume. If any of these policies are violated, the OS may
decline to extend the memory of the process.
The idea of breaking a program into segments is so powerful and use-
ful that it was common for many decades to have the concept implemented
in hardware. (If you have taken a class in computer architecture and op-
erating systems, you have probably studied this in some detail.) The basic
idea is that the CPU maintains a table of segments, recording the starting
address and length, along with the permissions associated with each seg-
ment. The operating system would typically set up a hardware segment
to correspond to the logical organization just described.
Although hardware segmentation was widely used in operating sys-
tems through the 1980s, it has been largely replaced by paging, which
was seen as simpler and more flexible. Processor vendors have responded
by removing support for hardware segmentation in new designs. For ex-
ample, every generation of the Intel X86 architecture from the 8086 up to
the Pentium supported segmentation in 32-bit protected mode. The latest
64-bit architectures provide only paging facilities, and no segmentation.
Logical segmentation continues as a useful way to organize programs in
memory.
Let’s continue by looking at each of the logical segments in more detail.

### 9.3 Heap Management

The heap contains memory that is managed dynamically at runtime. The
OS does not control the internal organization of the heap, except to limit
its total size. Instead, the internal structure of the heap is managed by the
standard library or other runtime support software that is automatically
linked into a program. In a C program, the functions **malloc** and **free** al-
locate and release memory on the heap, respectively. In C++, **new** and
**delete** have the same effect. Other languages manipulate the heap implic-
itly when objects and arrays are created and deleted.
The simplest implementation of **malloc** and **free** is to treat the entire
heap as one large linked list of memory regions. Each entry in the list
records the state of the region (free or in use), the size of the region, and
has pointers to the previous and next regions. Here’s what that might look
like in C:

struct chunk {
enum { FREE, USED } state;
int size;
struct chunk *next;
struct chunk *prev;
char data[0];
};


##### 9.3. HEAP MANAGEMENT 139

(Note that we declareddataas an array of length zero. This is a little
trick that allows us to treatdataas a variable length array, provided that
the underlying memory is actually present.)
Under this scheme, the initial state of the heap is simply one entry in a
linked list:

```
FREE 1000 data
prev next
```
Suppose that the user callsmalloc(100)to allocate 100 bytes of mem-
ory.mallocwill see that the (single) chunk of memory is free, but much
larger than the requested size. So, it will split it into one small chunk of
100 bytes and one larger chunk with the remainder. This is accomplished
by simply writing a new chunk header into the data area after 100 bytes.
Then, connect them together into a linked list:

```
USED 100 data FREE 900 data
prev next prev next
```
Once the list has been modified,mallocreturns the address of the
data element within the chunk, so that the user can access it directly. It
doesn’t return the linked list node itself, because the user doesn’t need to
know about the implementation details. If there is no chunk available that
is large enough to satisfy the current request, then the process must ask
the OS to extend the heap by callingbrk.
When the user callsfreeon a chunk of memory, the state of the chunk
in the linked list is markedFREE, and then merged with adjacent nodes, if
they are also free.
(Incidentally, now you can see why it is dangerous for a program to
modify memory carelessly outside a given memory chunk. Not only could
it affect other chunks, but it could damage the linked list itself, resulting
in wild behavior on the nextmallocorfree!)
If the program always frees memory in the opposite order that it was
allocated, then the heap will be nicely split into allocated and free memory.
However, that isn’t what happens in practice: memory can be allocated
and freed in any order. Over time, the heap can degenerate into a mix
of oddly sized chunks of allocated and freed memory. This is known as
**memory fragmentation**.
Excessive fragmentation can result in waste: if there are many small
chunks available, but none of them large enough to satisfy the current
malloc, then the process has no choice but to extend the heap, leaving
the small pieces unused. This increases pressure on total virtual memory
consumption in the operating system.
In a language like C, memory chunks cannot be moved while in use,
and so fragmentation cannot be fixed after it has already occurred. How-


##### 140 CHAPTER 9. MEMORY ORGANIZATION

ever, the memory allocator has some limited ability to avoid fragmenta-
tion by choosing the location of new allocations with care. Some simple
strategies are easy to imagine and have been studied extensively:

- **Best Fit.** On each allocation, search the entire linked list and find the
    smallestfree chunk that is larger than the request. This tends to leave
    large spaces available, but generates tiny leftover free fragments that
    are too small to be used.
- **Worst Fit.** On each allocation, search the entire linked list and find
    thelargestfree chunk that is larger than the request. Somewhat coun-
    terintuitively, this method tends to reduce fragmentation by avoid-
    ing the creation of tiny unusable fragments.
- **First Fit.** On each allocation, search the linked list from the begin-
    ning, and find thefirstfragment (large or small) that satisfies the
    request. This performs less work than Best Fit or Worst Fit, but per-
    forms an increasing amount of work as the linked list increases in
    size.
- **Next Fit.** On each allocation, search the linked list from the last ex-
    amined location, and find thenextfragment (large or small) that sat-
    isfies the request. This minimizes the amount of work done on each
    allocation, while distributing allocations throughout the heap.

For general purpose allocators where one cannot make assumptions
about application behavior, the conventional wisdom is that Next Fit re-
sults in good performance with an acceptable level of fragmentation.

### 9.4 Stack Management

The **stack** is used to record the current state of the running program. Most
CPUs have a specialized register – the **stack pointer** – which stores the
address where the next item will be pushed or popped. Because the stack
grows down from the top of memory, there is a confusing convention:
pushing an item on the stack causes the stack pointer to move to a lower
numbered address, while popping an item off the stack causes the stack
pointer to move to a higher address. The “top” of the stack is actually at
the lowest address!
Each invocation of a function occupies a range of memory in the stack,
known as a **stack frame**. The stack frame contains the parameters and
the local variables used by that function. When a function is called, a
new stack frame is pushed; when the function returns, the stack frame
is popped, and execution continues in the caller’s stack frame.
Another specialized register known as the **frame pointer** (or some-
times **base pointer** ) indicates the beginning of the current frame. Code


##### 9.4. STACK MANAGEMENT 141

within a function relies upon the frame pointer to identify the location of
the current parameters and local variables.
For example, suppose that themainfunction calls functionf, and then
fcallsg. If we stop the program in the middle of executingg, the stack
would look like this:

```
Stack Frame Parameters tomain
formain: Old Frame Pointer
Local Variables
Return Address
Stack Frame Parameters tof
forf: Old Frame Pointer
Local Variables
Return Address
Stack Frame Parameters tog ←Frame Pointer
forg: Old Frame Pointer
Local Variables ←Stack Pointer
↓(stack grows down)↓
```
The order and details of the elements in a stack frame differ somewhat
between CPU architectures and operating systems. As long as both the
caller and the callee agree on what goes in the stack frame, then any func-
tion may call another, even if they were written in different languages, or
built by different compilers.
The agreement on the contents of the activation record is known as a
**calling convention**. This is typically written out in a detailed technical
document that is used by the designers of compilers, operating systems,
and libraries to ensure that code is mutually interoperable.
There are two broad categories of calling conventions, with many op-
portunities for variation in between. One is to put the arguments to a
function call on the stack, and the other is to place them in registers.

#### 9.4.1 Stack Calling Convention

The conventional approach to calling a function is to push the arguments
to that function on the stack (in reverse order), and then to jump to the
address of the function, leaving behind a return address on the stack. Most
CPUs have a specializedCALLinstruction for this purpose. For example,
the assembly code to callf(10,20)could be as simple as this:

PUSH $20
PUSH $10
CALL f

Whenfbegins executing, it saves the old frame pointer currently in
effect and makes space for its own local variables. As a result, the stack
frame forf(10,20)looks like this:


##### 142 CHAPTER 9. MEMORY ORGANIZATION

```
2nd Argument (20)
1st Argument (10)
Return Address
Old Frame Pointer ←Frame Pointer
```
```
Local Variables
←Stack Pointer
↓(stack grows down)↓
```
To access its arguments or local variables,fmust load them from mem-
ory relative to the frame pointer. As you can see, the function arguments
are found at fixed positionsabovethe frame pointer, while local variables
are foundbelowthe frame pointer.^2

#### 9.4.2 Register Calling Convention

An alternate approach to calling a function is to put the arguments into
registers, and then call the function. For example, let us suppose that our
calling convention indicates that registers %R10, %R11, etc are to be used
for arguments. Under this calling convention, the assembly code to invoke
f(10,20)might look like this:

##### MOVE $10 -> %R10

##### MOVE $20 -> %R11

CALL f

Whenfbegins executing, it still must save the old frame pointer and
make room for local variables. It doesn’t have to load arguments from the
stack; it simply expects the values in %R10 and %R11 and can compute
on them right away. This could confer a significant speed advantage by
avoiding memory accesses.

But, what iffis a complex function that needs to invoke other func-
tions? It will still need to save the current values of the argument registers,
in order to free them up for its own use.

To allow for this possibility, the stack frame forfmust leave space for
the arguments, in case they must be saved. The calling convention must
define the location of the arguments, and they are typically storedbelow
the return address and old frame pointer, like this:

(^2) The arguments are pushed in reverse order in order to allow the possibility of a variable
number of arguments. Argument 1 is always two words above the frame pointer, argument
2 is always three words above, and so on.


##### 9.5. LOCATING DATA 143

```
Return Address
Old Frame Pointer ←Frame Pointer
1st Argument (10)
2nd Argument (20)
```
Local Variables
←Stack Pointer
↓(stack grows down)↓
What happens if the function has more arguments than there are reg-
isters set aside for arguments? In this case, the additional arguments are
pushed on to the stack, as in the stack calling convention.
In the big picture, the choice between stack and register calling conven-
tions doesn’t matter much, except that all parties must agree on the details
of the convention. The register calling convention has the slight advan-
tage that a **leaf function** (a function that does not call other functions) can
compute a simple result without accessing memory. Typically, the regis-
ter calling convention is used on architectures that have a large number of
registers that might otherwise go unused.
It is possible to mix the conventions in a single program, as long as
both caller and callee are informed of the distinction. For example, the
Microsoft X86 compilers allow keywords in function prototypes to select a
convention:cdeclselects the stack calling convention, whilefastcall
uses registers for the first two arguments.

### 9.5 Locating Data

For each kind of data in a program, there must be an unambiguous method
of locating that data in memory. The compiler must generate an **address
computation** using the basic information available about the symbol. The
computation varies with the storage class of the data:

- **Global data** has the easiest address computation. In fact, the com-
    piler doesn’t usually compute global addresses, but rather passes the
    nameof each global symbol to the assembler, which then selects the
    address computation. In the simplest case, the assembler will gen-
    erate an **absolute address** giving the exact location of the data in
    program memory.
    However, the simple approach isn’t necessarily efficient, because an
    absolute address is a full word (e.g. 64 bits), the same size as an
    instruction in memory. This means that the assembler must use sev-
    eral instructions (RISC) or multi-word instructions (CISC) to load the
    address into a register. Assuming that most programs don’t use the
    entire address space, it isn’t usually necessary to use the entire word.
    An alternative is to use a **base-relative address** that consists of a base
    address given by a register plus a fixed offset given by the assembler.


##### 144 CHAPTER 9. MEMORY ORGANIZATION

```
For example, global data addresses could be given by a register in-
dicating the beginning of the data segment, plus a fixed offset, while
a function address could be given by a register indicating the begin-
ning of the code segment plus a fixed offset. Such an approach can
be used in dynamically loaded libraries, when the location of the li-
brary is not fixed in advance, but the location of a function within
the library is known.
Yet another approach is to use a PC-relative address in which the
exact distance in bytes between the referring instruction and the tar-
get data is computed, and then encoded into the instruction. This
works as long as the relative distance is small enough (e.g. 16 bits)
to fit into the address field of the instruction. This task is performed
by the assembler and is usually invisible to the programmer.
```
- **Local data** works differently. Because local variables are stored on
    the stack, a given local variable does not necessarily occupy the same
    absolute address each time it is used. If a function is called recur-
    sively, there may be multiple instances of a given local variable in
    use simultaneously! For this reason, local variables are always speci-
    fied as an offset relative to the current frame pointer. (The offset may
    be positive or negative, depending on the function calling conven-
    tion.) Function parameters are just a special case of local variables:
    a parameter’s position on the stack is given precisely by its ordinal
    position in the parameters.
- **Heap data** can only be accessed by way of pointers that are stored
    as global or local variables. To access data on the heap, the compiler
    must generate an address computation for the pointer itself, then de-
    reference the pointer to reach the item on the heap.

So far, we have only considered atomic data types that are easily stored
in a single word of memory: booleans, integers, floats, and so forth. How-
ever, any of the more complex data types can be placed in any of the three
storage classes, and require some additional handling.
An array can be stored in global, local, or heap memory, and the be-
ginning of the array is found by one of the methods above. An element in
the array is found by multiplying the index by the size of the items in the
array, and adding that to the address of the array itself:

address(a[i]) = address(a) + sizeof(type) * i

The more interesting question is how to deal with the length of the
array itself. In an unsafe language like C, the simple approach is to simply
do nothing: if the program happens to run off the end of the array, the
compiler will happily compute an address outside the array bounds, and
chaos results. For some applications where performance is paramount, the
simplicity of this approach trumps any increase in safety.


##### 9.5. LOCATING DATA 145

A safer approach is to store the length of the array at the base address
of the array itself. Then, the compiler may generate code that checks the
actual index against the array bounds before generating the address. This
prevents any sort of runtime accident by the programmer. However, the
downside is performance. Every time the programmer mentionsa[i],
the resulting code must contain this:

1. Compute address of arraya.
2. Load length ofainto a register.
3. Compare array indexito register.
4. Ifiis outside of array bounds, raise an exception.
5. Otherwise, compute address ofa[i]and continue.

This pattern is so common that some computer architectures provide
dedicated support for array bounds checking. The Intel X86 architecture,
(which we will examine in detail in the next chapter) provides a unique
BOUNDinstruction, whose only purpose is to compare a value against two
array bound values, and then raise a unique “Array Bounds Exception” if
it falls outside.
Structures have similar considerations. In memory, a structure is very
much like an array, except that it can contain items of irregular size. To
access an item within a structure, the compiler must generate an address
computation of the beginning of the structure, and then add an offset cor-
responding to the name of item (known as the **structure tag** ) within the
structure. Of course, it is not necessary to perform bounds checking since
the offsets are fixed at compile time.
For complex nested data structures, the address computation necessary
to find an individual element can become quite complicated. For example,
consider this bit of code to represent a deck of cards:

struct card {
int suit;
int rank;
};

struct deck {
int is_shuffled;
struct card cards[52];
};

struct deck d;

d.cards[10].rank = 10;


##### 146 CHAPTER 9. MEMORY ORGANIZATION

To computed.cards[10].rank, the compiler must first generate an
address computation ford, depending on whether it is a local or global
variable. From there, the offset ofcardsis added, then the offset of the
tenth item, then the offset ofrankwithin the card. The complete address
computation is:

address(d.card[10].rank) =
address(d)
+ offset(cards)
+ sizeof(struct card)* 10
+ offset(rank)

### 9.6 Program Loading

Before a program begins executing in memory, it first exists as a file on
disk, and there must be a convention for loading it into memory. There are
a variety of **executable formats** for organizing a program on disk, ranging
from very simple to very complex. Here are a few examples to give you
the idea.
The simplest computer systems simply store an executable as a **binary
blob** on disk. The program code, data, and initial state of the heap and
stack are simply dumped into one file without distinction. To run the pro-
gram, the OS must simply load the contents of the file into memory, and
then jump to the first location of the program to begin execution.
This approach is about as simple as one can imagine. It does work, but
it has several limitations. One is that the format wastes space on uninitial-
ized data. For example, if the program declares a large global array where
each element has the value zero, then every single zero in that array will
be stored in the file. Another is that the OS has no insight into how the
program intends to use memory, so it is unable to set permissions on each
logical segment, as discussed above. Yet another is that the binary blob
has no identifying information to show that it is an executable.
However, the binary blob approach is still occasionally used in places
where programs are small and simplicity is paramount. For example, the
very first boot stage of a PC operating system reads in a single sector from
the boot hard disk containing a binary blob, which then carries out the sec-
ond stage of booting. Embedded systems often have very small programs
measured in a few kilobytes, and rely on binary blobs.
An improved approach used in classic Unix systems for many years
is the **a.out** executable format.^3 There are many slight variations on the
format, but they all share the same basic structure. The executable file
consists of a short header structure, followed by the text, initialized data,
and symbol table:

(^3) The first Unix assembler sent its output to a file nameda.outby default. In the absence
of any other name for the format, the name stuck.


##### 9.6. PROGRAM LOADING 147

```
Header Text Data Symbols
```
The header structure itself is just a few bytes that allow the operating
system to interpret the rest of the file:

```
Magic Number
Text Section Size
Data Section Size
BSS Size
Symbol Table Size
Entry Point
```
The **magic number** is a unique integer that clearly defines the file as an
executable: if the file does not begin with this magic number, the OS will
not even attempt to execute it. Different magic numbers are defined for
executables, unlinked object files, and shared libraries. The **text size** field
indicates the number of bytes in the text section that follows the header.
The **data size** field indicates the amount ofinitializeddata that appears in
the file, while the **BSS size** field indicates the amount ofuninitialized data.
4

The uninitialized data need not be stored in the file. Instead it is simply
allocated in memory as part of the data segment when the program is
loaded. The **symbol table** in the executable lists each of the variable and
function names used in the program along with their locations in the code
and data segment; this permits a debugger to interpret the meaning of
addresses. Finally, the **entry point** gives the address of the starting point of
the program (typicallymain) in the text segment. This allows the starting
point to be something other than the first address in the program.

Thea.outformat is a big improvement over a binary blob, and is still
supported and usable today in many operating systems. However, it isn’t
quite powerful enough to support many of the features needed by modern
languages, particularly dynamically loaded libraries.

The **Extensible Linking Format (ELF)** is widely used today across many
operating systems to represent executables, object files, and shared libraries.
Likea.out, an ELF file has multiple sections representing code, data, and
bss, but it can have an arbitrary number of additional sections for debug-
ging data, initialization and finalization code, metadata about the tools
used, and so forth. The number ofsectionsin the file outnumbers theseg-
mentsin memory, and so a **section table** in the ELF file indicates how mul-
tiple sections are to be mapped into a single segment.

(^4) BSS stands for “Block Started by Symbol” and first appeared in an assembler for IBM 704
in the 1950s.


##### 148 CHAPTER 9. MEMORY ORGANIZATION

```
File Header
Program Header
Code Section
Data Section
Read-Only Section
```
```
Section Header
```
### 9.7 Further Reading

1. Remzi Arpaci-Dusseau and Andrea Arpaci-Dusseau, “Operating Sys-
    tems: Three Easy Pieces”, Arpaci-Dusseau Books, 2015.
    [http://www.ostep.org](http://www.ostep.org)
    Operating systems is usually the course in which memory management is
    covered in great detail. If you need a refresher on memory allocators (or
    anything else in operating systems), check out this online textbook.
2. John R. Levine, “Linkers and Loaders”, Morgan Kaufmann, 1999.
    This book provides a detailed look at linkers and loaders, which is an often-
    overlooked topic that falls in cracks between compilers and operating sys-
    tems. A solid understanding of linking is necessarily to create and use
    libraries effectively.
3. Paul R. Wilson, “Uniprocessor Garbage Collection Techniques”, Lec-
    ture Notes in Computer Science, volume 637, 1992.
    https://link.springer.com/chapter/10.1007/BFb0017182
    This widely-read article gives an accessible overview of the key techniques
    of garbage collection, which is an essential component of the runtime of
    modern dynamic languages.


##### 149

## Chapter 10 – Assembly Language

### 10.1 Introduction

In order to build a compiler, you must have a working knowledge of at
least one kind of assembly language. And, it helps to see two or more
variations of assembly, so as to fully appreciate the distinctions between
architectures. Some of these differences, such as register structure, are
quite fundamental, while some of the differences are merely superficial.
We have observed that many students seem to think that assembly lan-
guage is rather obscure and complicated. Well, it is true that the complete
manual for a CPU is extraordinarily thick, and may document hundreds
of instructions and obscure addressing modes. However, it’s been our ex-
perience that it is really only necessary to learn a small subset of a given
assembly language (perhaps 30 instructions) in order to write a functional
compiler. Many of the additional instructions and features exist to handle
special cases for operating systems, floating point math, and multi-media
computing. You can do almost everything needed with the basic subset.
We will look at two different CPU architectures that are in wide use to-
day: X86 and ARM. The Intel X86 is a CISC architecture that has evolved
since the 1970s from 8-bit to 64-bit and is now the dominant chip in per-
sonal computers, laptops, and high performance servers. The ARM pro-
cessor is a RISC architecture that began life as a 32-bit chip for the personal
computer market, and is now the dominant chip for low-power and em-
bedded devices such as mobile phones and tablets.
This chapter will give you a working knowledge of the basics of each
architecture, but you will need a good reference to look up more details.
We recommend that you consult the Intel Software Developer Manual[1]
and the ARM Architecture Reference Manual[3]for the complete details.
(Note that each section is meant to be parallel and self-contained, so some
explanatory material is repeated for both X86 and ARM.)


##### 150 CHAPTER 10. ASSEMBLY LANGUAGE

### 10.2 Open Source Assembler Tools

A given assembly language can have multiple dialects for the same CPU,
depending on whether one uses the assembler provided by the chip ven-
dor, or other open source tools. For consistency, we will give examples
in the assembly dialect supported by the GNU compiler and assembler,
which are known asgccandas(or sometimesgas).
A good way to get started is to view the assembler output of the com-
piler for a C program. To do this, rungccwith the-Sflag, and the
compiler will produce assembly output rather than a binary program. On
Unix-like systems, assembly code is stored in files ending with.s, which
indicates “source” file.
If you rungcc -S hello.c -o hello.son this C program:

#include <stdio.h>

int main( int argc, char *argv[] )
{
printf("hello %s\n","world");
return 0;
}

then you should see output similar to this inhello.s

.file "test.c"
.data
.LC0:
.string "hello %s\n"
.LC1:
.string "world"
.text
.global main
main:
PUSHQ %rbp
MOVQ %rsp, %rbp
SUBQ $16, %rsp
MOVQ %rdi, -8(%rbp)
MOVQ %rsi, -16(%rbp)
MOVQ $.LC0, %rax
MOVQ $.LC1, %rsi
MOVQ %rax, %rdi
MOVQ $0, %rax
CALL printf
MOVQ $0, %rax
LEAVE
RET


##### 10.2. OPEN SOURCE ASSEMBLER TOOLS 151

(There are many valid ways to compilehello.cand so the output of
your compiler may be somewhat different.)
Regardless of the CPU architecture, the assembly code has three differ-
ent kinds of elements:
**Directives** begin with a dot and indicate structural information use-
ful to the assembler, linker, or debugger, but are not in and of themselves
assembly instructions. For example,.filesimply records the name of
the original source file to assist the debugger..dataindicates the start of
the data segment of the program, while.textindicates the start of the
program segment. .stringindicates a string constant within the data
section, and.global mainindicates that the labelmainis a global sym-
bol that can be accessed by other code modules.
**Labels** end with a colon and indicate by their position the association
between names and locations. For example, the label.LC0:indicates that
the immediately following string should be called.LC0. The labelmain:
indicates that the instructionPUSHQ %rbpis the first instruction of the
main function. By convention, labels beginning with a dot are temporary
local labels generated by the compiler, while other symbols are user-visible
functions and global variables. The labels do not become part of the result-
ing machine codeper se, but they are present in the resulting object code
for the purposes of linking, and in the eventual executable, for purposes
of debugging.
**Instructions** are the actual assembly code like (PUSHQ %rbp), typically
indented to visually distinguish them from directives and labels. Instruc-
tions in GNU assembly are not case sensitive, but we will generally upper-
case them, for consistency.
To take thishello.sand turn it into a runnable program, just run
gcc, which will figure out that it is an assembly program, assemble it, and
link it with the standard library:

% gcc hello.s -o hello
% ./hello
hello world

It is also interesting to compile the assembly code into object code, and
then use thenmutility to display the symbols (”names”) present in the
code:

% gcc hello.s -c -o hello.o
% nm hello.o
0000000000000000 T main
U printf

This displays the information available to the linker.mainis present in
the text (T) section of the object, at location zero, andprintfis undefined


##### 152 CHAPTER 10. ASSEMBLY LANGUAGE

(U), since it must be obtained from the standard library. But none of the
labels like.LC0appear because they were not declared as.global.
As you are learning assembly language, take advantage of an existing
compiler: write some simple functions to see whatgccgenerates. This can
give you some starting points to identify new instructions and techniques
to use.

### 10.3 X86 Assembly Language

X86 is a generic term that refers to the series of microprocessors descended
from (or compatible with) the Intel 8088 processor used in the original IBM
PC, including the 8086, 80286, ’386, ’486, and many others. Each genera-
tion of CPUs added new instructions and addressing modes from 8-bit to
16-bit to 32-bit, all while retaining backwards compatibility with old code.
A variety of competitors (such as AMD) produced compatible chips that
implemented the same instruction set.
However, Intel broke with tradition in the 64-bit generation by intro-
ducing a new brand (Itanium) and architecture (IA64) that wasnotback-
wards compatible with old code. Instead, it implemented a new concept
known as Very Long Instruction Word (VLIW) in which multiple concur-
rent operations were encoded into a single word. This had the potential for
significant speedups due to instruction-level parallelism but represented
a break with the past.
AMD stuck with the old ways and produced a 64-bit architecture
(AMD64) thatwasbackwards compatible with both Intel and AMD chips.
While the technical merits of both approaches were debatable, the AMD
approach won in the marketplace, and Intel followed by producing its
own 64-bit architecture (Intel64) that was compatible with AMD64 and its
own previous generation of chips. X86-64 is the generic name that covers
both AMD64 and Intel64 architectures.
X86-64 is a fine example of CISC (complex instruction set computing).
There are a very large number of instructions with many different sub-
modes, some of them designed for very narrow tasks. However, a small
subset of instructions will let us accomplish a lot.

#### 10.3.1 Registers and Data Types

X86-64 has sixteen (almost) general purpose 64-bit integer registers:

```
%rax %rbx %rcx %rdx %rsi %rdi %rbp %rsp
%r8 %r9 %r10 %r11 %r12 %r13 %r14 %r15
```
These registers arealmostgeneral purpose because earlier versions of
the processors intended for each register to be used for a specific purpose,
and not all instructions could be applied to every register. The names of


##### 10.3. X86 ASSEMBLY LANGUAGE 153

```
A Note on AT&T Syntax versus Intel Syntax
Note that the GNU tools use the traditional AT&T syntax, which is used
across many processors on Unix-like operating systems, as opposed to the
Intel syntax typically used on DOS and Windows systems. The following
instruction is given in AT&T syntax:
```
```
MOVQ %RSP, %RBP
```
```
MOVQis the name of the instruction, and the percent signs indicate that
RSPandRBPare registers. In the AT&T syntax, the source is always given
first, and the destination is always given second.
```
```
In other places (such as the Intel manual), you will see the Intel syntax,
which (among other things) dispenses with the percent signs andreverses
the order of the arguments. For example, this is the same instruction in the
Intel syntax:
```
```
MOVQ RBP, RSP
```
```
When reading manuals and web pages, be careful to determine whether
you are looking at AT&T or Intel syntax: look for the percent signs!
```
the lower eight registers indicate the purpose for which each was origi-
nally intended: for example, %rax is the accumulator.

As the design developed, new instructions and addressing modes were
added to make the various registers almost equal. A few remaining in-
structions, particularly related to string processing, require the use of %rsi
and %rdi. In addition, two registers are reserved for use as the stack
pointer (%rsp) and the base pointer (%rbp). The final eight registers are
numbered and have no specific restrictions.

The architecture has expanded from 8 to 64 bits over the years, and so
each register has some internal structure. The lowest 8 bits of the %rax
register are an 8-bit register %al, and the next 8 bits are known as %ah.
The low 16 bits are collectively known as %ax, the low 32-bits as %eax,
and the whole 64 bits as %rax.

```
Figure 10.1: X86 Register Structure
```
```
8 bits: al ah
16 bits: ax
32 bits: eax
64 bits: rax
```

##### 154 CHAPTER 10. ASSEMBLY LANGUAGE

The numbered registers %r8-%r15 have the same structure, but a slightly
different naming scheme:

```
8 bits: r8b
16 bits: r8w
32 bits: r8d
64 bits: r8
```
```
Figure 10.2: X86 Register Structure
```
To keep things simple, we will focus our attention on the 64-bit regis-
ters. However, most production compilers use a mix of modes: a byte can
represent a boolean; a longword is usually sufficient for integer arithmetic,
since most programs don’t need integer values above 232 ; and a quadword
is needed to represent a memory address, enabling up to 16EB (exa-bytes)
of virtual memory.

#### 10.3.2 Addressing Modes

TheMOVinstruction moves data between registers and to and from mem-
ory in a variety of different modes. A single letter suffix determines the
size of data to be moved:

```
Suffix Name Size
B BYTE 1 byte (8 bits)
W WORD 2 bytes (16 bits)
L LONG 4 bytes (32 bits)
Q QUADWORD 8 bytes (64 bits)
```
MOVBmoves a byte,MOVWmoves a word,MOVLmoves a long,MOVQ
moves a quad-word.^1 Generally, the size of the locations you are moving
to and from must match the suffix. In some cases, you can leave off the
suffix, and the assembler will infer the right size. However, this can have
unexpected consequences, so we will make a habit of using the suffix.
The arguments to MOV can have one of several addressing modes.

- A **global value** is simply referred to by an unadorned name such asx
    orprintf, which the assembler translates into an absolute address
    or an address computation.
- An **immediate value** is a constant value indicated by a dollar sign
    such as$56, and has a limited range, depending on the instruction
    in use.
- A **register value** is the name of a register such as%rbx.

(^1) Careful: These terms are not portable. Awordhas a different size on different machines.


##### 10.3. X86 ASSEMBLY LANGUAGE 155

- An **indirect value** refers to a value by the address contained in a
    register. For example,(%rsp)refers to the value pointed to by %rsp.
- A **base-relative** value is given by adding a constant to the name of
    a register. For example,-16(%rcx)refers to the value at the mem-
    ory location sixteen bytes below the address indicated by%rcx. This
    mode is important for manipulating stacks, local values, and func-
    tion parameters, where the start of an object is given by a register.
- A **complex** address is of the formD(RA,RB,C)which refers to the
    value at addressRA+RB∗C+D. BothRAandRBare general
    purpose registers, whileCcan have the value 1, 2, 4, or 8, andD
    can be any integer displacement. This mode is used to select an item
    within an array, whereRAgives the base of the array,RBgives the
    index into the array,Cgives the size of the items in the array, andD
    is an offset relative to that item.

Here is an example of using each kind of addressing mode to load a
64-bit value into%rax:

```
Mode Example
Global Symbol MOVQ x, %rax
Immediate MOVQ $56, %rax
Register MOVQ %rbx, %rax
Indirect MOVQ (%rsp), %rax
Base-Relative MOVQ -8(%rbp), %rax
Complex MOVQ -16(%rbx,%rcx,8), %rax
```
For the most part, the same addressing modes may be used to store
data into registers and memory locations. There are some exceptions. For
example, it is not possible to use base-relative for both arguments of MOV:
MOVQ -8(%rbx), -8(%rbx). To see exactly what combinations of ad-
dressing modes are supported, you must read the manual pages for the
instruction in question.
In some cases, you may want to load theaddressof a variable instead of
its value. This is handy when working with strings or arrays. For this pur-
pose, use theLEA(load effective address) instruction, which can perform
the same address computations asMOV:

```
Mode Example
Global Symbol LEAQ x, %rax
Base-Relative LEAQ -8(%rbp), %rax
Complex LEAQ -16(%rbx,%rcx,8), %rax
```

##### 156 CHAPTER 10. ASSEMBLY LANGUAGE

#### 10.3.3 Basic Arithmetic

You will need four basic arithmetic instructions for your compiler: integer
addition, subtraction, multiplication, and division.
ADD and SUB have two operands: a source and a destructive target.
For example, this instruction:

ADDQ %rbx, %rax

adds %rbx to %rax, and places the result in %rax, overwriting what might
have been there before. This requires a little care, so that you don’t acci-
dentally clobber a value that you might want to use later. For example,
you could translatec = a+b+b;like this:

MOVQ a, %rax
MOVQ b, %rbx
ADDQ %rbx, %rax
ADDQ %rbx, %rax
MOVQ %rax, c

TheIMULinstruction is a little unusual, because multiplying two 64-
bit integers results in a 128-bit integer, in the general case.IMULtakes its
argument, multiplies it by the contents of %rax, and then places the low
64 bits of the result in %rax and the high 64 bits in %rdx. (This is implicit:
%rdx is not mentioned in the instruction.)
For example, suppose that you wish to translatec = b*(b+a);, where
a, andb, andcare global integers. Here is one possible translation:

MOVQ a, %rax
MOVQ b, %rbx
ADDQ %rbx, %rax
IMULQ %rbx
MOVQ %rax, c

The IDIV instruction does the same thing, except backwards: it starts
with a 128 bit integer value whose low 64 bits are in %rax and high 64 bits
in %rdx, and divides it by the value given in the instruction. The quotient
is placed in %rax and the remainder in %rdx. (If you want to implement
the modulus instruction instead of division, just use the value of %rdx.)
To set up a division, you must make sure that both registers have the
necessary sign-extended value. If the dividend fits in the lower 64 bits, but
is negative, then the upper 64 bits must all be ones to complete the twos-
complement representation. The CQO instruction serves the very specific
purpose of sign-extending %rax into %rdx for division.
For example, to divide a by five:


##### 10.3. X86 ASSEMBLY LANGUAGE 157

MOVQ a, %rax # set the low 64 bits of the dividend
CQO # sign-extend %rax into %rdx
IDIVQ $5 # divide %rdx:%rax by 5,
# leaving result in %rax

The instructions INC and DEC increment and decrement a register de-
structively. For example, the statementa = ++bcould be translated as:

MOVQ b, %rax
INCQ %rax
MOVQ %rax,b
MOVQ %rax, a

The instructions AND, OR, and XOR perform destructivebitwiseboolean
operations on two values. Bitwise means that the operation is applied to
each individual bit in the operands, and stored in the result.
So,AND $0101B $0110Bwould yield the result$0100B. In a similar
way, the NOT instruction inverts each bit in the operand. For example,
the bitwise C expressionc = (a & ̃b);could be translated like this:

MOVQ a, %rax
MOVQ b, %rbx
NOTQ %rbx
ANDQ %rax, %rbx
MOVQ %rbx, c

Be careful here: these instructionsdo notimplement logical boolean op-
erations according to the C representation that you are probably familiar
with. For example, if you define “false” to be the integer zero, and “true”
to be any non-zero value. In that case,$0001is true, butNOT $0001Bis
$1110B, which is also true! To implement that correctly, you need to use
CMP with conditionals described below.^2
Like the MOV instruction, the various arithmetic instructions can work
on a variety of addressing modes. However, for your compiler project, you
will likely find it most convenient to use MOV to load values in and out of
registers, and then use only registers to perform arithmetic.

(^2) Alternatively, you could could use the bitwise operators as logical operators if you give
truethe integer value -1 (all ones) andfalsethe integer value zero.


##### 158 CHAPTER 10. ASSEMBLY LANGUAGE

#### 10.3.4 Comparisons and Jumps

Using the JMP instruction, we may create a simple infinite loop that counts
up from zero using the %rax register:

MOVQ $0, %rax
loop: INCQ %rax
JMP loop

To define more useful structures such as terminating loops and if-then
statements, we must have a mechanism for evaluating values and chang-
ing program flow. In most assembly languages, these are handled by two
different kinds of instructions: compares and jumps.
All comparisons are done with the CMP instruction. CMP compares
two different registers and then sets a few bits in the internal EFLAGS
register, recording whether the values are the same, greater, or lesser. You
don’t need to look at the EFLAGS register directly. Instead a selection of
conditional jumps examine the EFLAGS register and jump appropriately:

```
Instruction Meaning
JE Jump if Equal
JNE Jump if Not Equal
JL Jump if Less
JLE Jump if Less or Equal
JG Jump if Greater
JGE Jump if Greater or Equal
```
```
For example, here is a loop to count %rax from zero to five:
```
MOVQ $0, %rax
loop: INCQ %rax
CMPQ $5, %rax
JLE loop

And here is a conditional assignment: if global variable x is greater
than zero, then global variable y gets ten, else twenty:

MOVQ x, %rax
CMPQ $0, %rax
JLE .L1
.L0:
MOVQ $10, $rbx
JMP .L2
.L1:
MOVQ $20, $rbx
.L2:
MOVQ %rbx, y


##### 10.3. X86 ASSEMBLY LANGUAGE 159

Note that jumps require the compiler to define target labels. These
labels must be unique and private within one assembly file, but cannot be
seen outside the file unless a.globaldirective is given. Labels like.L0,
.L1, etc, can be generated by the compiler on demand.

#### 10.3.5 The Stack

The stack is an auxiliary data structure used primarily to record the func-
tion call history of the program along with local variables that do not fit
in registers. By convention, the stack growsdownwardfrom high values
to low values. The %rsp register is known as the **stack pointer** and keeps
track of the bottom-most item on the stack.
To push %rax onto the stack, we must subtract 8 (the size of %rax in
bytes) from %rsp and then write to the location pointed to by %rsp:

SUBQ $8, %rsp
MOVQ %rax, (%rsp)

```
Popping a value from the stack involves the opposite:
```
MOVQ (%rsp), %rax
ADDQ $8, %rsp

To discard the most recent value from the stack, just move the stack
pointer the appropriate number of bytes :

ADDQ $8, %rsp

Of course, pushing to and popping from the stack referred to by %rsp
is so common, that the two operations have their own instructions that
behave exactly as above:

PUSHQ %rax
POPQ %rax

Note that, in 64-bit code,PUSHandPOPare limited to working with
64-bit values, so a manualMOVandADDmust be used if it is necessary to
move smaller items to/from the stack.


##### 160 CHAPTER 10. ASSEMBLY LANGUAGE

#### 10.3.6 Calling a Function

Prior to the 64-bit architecture described here, a simple stack calling con-
vention was used: arguments were pushed on the stack in reverse order,
then the function was invoked withCALL. The called function looked for
the arguments on the stack, did its work, and returned the result in%eax.
The caller then removed the arguments from the stack.
However, 64-bit code uses a register calling convention, in order to ex-
ploit the larger number of available registers in the X86-64 architecture.^3
This convention is known as the **System V ABI** [2]and is written out in
a lengthy technical document. The complete convention is quite compli-
cated, but this summary handles the basic cases:

```
Figure 10.3: Summary of System V ABI Calling Convention
```
- The first six integer arguments (including pointers and other
    types that can be stored as integers) are placed in the registers
    %rdi, %rsi, %rdx, %rcx, %r8, and %r9, in that order.
- The first eight floating point arguments are placed in the registers
    %xmm0-%xmm7, in that order.
- Arguments in excess of those registers are pushed onto the stack.
- If the function takes a variable number of arguments (like
    printf) then the %rax register must be set to the number of
    floating point arguments.
- The return value of the function is placed in %rax.

In addition, we also need to know how the remaining registers are han-
dled. A few are **caller saved** , meaning that the calling function must save
those values before invoking another function. Others are **callee saved** ,
meaning that a function, when called, must save the values of those regis-
ters, and restore them on return. The argument and result registers need
not be saved at all. Figure 10.4 shows these requirements.
To invoke a function, we must first compute the arguments and place
them in the desired registers. Then, we must push the two caller-saved
registers (%r10and%r11) on the stack, to save their values. We then issue
theCALLinstruction, which pushes the current instruction pointer on to
the stack then jumps to the code location of the function. Upon return
from the function, we pop the two caller-saved registers off of the stack,
and look for the return value of the function in the%raxregister.

(^3) Note that there is nothingstoppingyou from writing a compiler that uses a stack call-
ing convention. But if you want to invoke functions compiled by others (like the standard
library) then you need to stick to the convention already in use.


##### 10.3. X86 ASSEMBLY LANGUAGE 161

```
Here is an example. The following C program:
```
int x=0;
int y=10;

int main()
{
x = printf("value: %d\n",y);
}

could be translated to this:

.data
x:
.quad 0
y:
.quad 10
str:
.string "value: %d\n"

.text
.global main
main:
MOVQ $str, %rdi # first argument in %rdi: string
MOVQ y, %rsi # second argument in %rsi: y
MOVQ $0, %rax # there are zero float args

```
PUSHQ %r10 # save the caller-saved regs
PUSHQ %r11
```
```
CALL printf # invoke printf
```
```
POPQ %r11 # restore the caller-saved regs
POPQ %r10
```
```
MOVQ %rax, x # save the result in x
```
```
RET # return from main function
```

##### 162 CHAPTER 10. ASSEMBLY LANGUAGE

```
Figure 10.4: System V ABI Register Assignments
```
```
Register Purpose Who Saves?
%rax result not saved
%rbx scratch callee saves
%rcx argument 4 not saved
%rdx argument 3 not saved
%rsi argument 2 not saved
%rdi argument 1 not saved
%rbp base pointer callee saves
%rsp stack pointer callee saves
%r8 argument 5 not saved
%r9 argument 6 not saved
%r10 scratch CALLER saves
%r11 scratch CALLER saves
%r12 scratch callee saves
%r13 scratch callee saves
%r14 scratch callee saves
%r15 scratch callee saves
```
#### 10.3.7 Defining a Leaf Function

Because function arguments are passed in registers, it is easy to write a
**leaf function** that computes a value without calling any other functions.
For example, code for the following function:

square: function integer ( x: integer ) =
{
return x*x;
}

```
Could be as simple as this:
```
.global square
square:
MOVQ %rdi, %rax # copy first argument to %rax
IMULQ %rax # multiply it by itself
# result is already in %rax
RET # return to caller

Unfortunately, this won’t work for a function that wants to invoke
other functions, because we haven’t set up the stack properly. A more
complex approach is needed for the general case.


##### 10.3. X86 ASSEMBLY LANGUAGE 163

#### 10.3.8 Defining a Complex Function

A complex function must be able to invoke other functions and compute
expressions of arbitrary complexity, and then return to the caller with the
original state intact. Consider the following recipe for a function that ac-
cepts three arguments and uses two local variables:

.global func
func:
pushq %rbp # save the base pointer
movq %rsp, %rbp # set new base pointer

```
pushq %rdi # save first argument on the stack
pushq %rsi # save second argument on the stack
pushq %rdx # save third argument on the stack
```
```
subq $16, %rsp # allocate two more local variables
```
```
pushq %rbx # save callee-saved registers
pushq %r12
pushq %r13
pushq %r14
pushq %r15
```
```
### body of function goes here ###
```
```
popq %r15 # restore callee-saved registers
popq %r14
popq %r13
popq %r12
popq %rbx
```
```
movq %rbp, %rsp # reset stack pointer
popq %rbp # recover previous base pointer
ret # return to the caller
```
There is a lot to keep track of here: the arguments given to the function,
the information necessary to return, and space for local computations. For
this purpose, we use the base register pointer %rbp. Whereas the stack
pointer %rsp points to the end of the stack where new data will be pushed,
the base pointer %rbp points to the start of the values used by this func-
tion. The space between %rbp and %rsp is known as the **stack frame** for
this function call.


##### 164 CHAPTER 10. ASSEMBLY LANGUAGE

There is one more complication: each function needs to use a selection
of registers to perform computations. However, what happens when one
function is called in the middle of another? We do not want any registers
currently in use by the caller to be clobbered by the called function. To
prevent this, each function must save and restore all of the registers that it
uses by pushing them onto the stack at the beginning, and popping them
off of the stack before returning. According to Figure 10.4, each function
must preserve the values of %rsp, %rbp, %rbx, and %r12-%r15 when it
completes.
Here is the stack layout forfunc, defined above:

```
Contents Address
old %rip register 8(%rbp)
old %rbp register (%rbp) ←%rbp points here
argument 0 -8(%rbp)
argument 1 -16(%rbp)
argument 2 -24(%rbp)
local variable 0 -32(%rbp)
local variable 1 -40(%rbp)
saved register %rbx -48(%rbp)
saved register %r12 -56(%rbp)
saved register %r13 -64(%rbp)
saved register %r14 -72(%rbp)
saved register %r15 -80(%rbp) ←%rsp points here
```
```
Figure 10.5: Example X86-64 Stack Layout
```
Note that the base pointer (%rbp) locates the start of the stack frame.
So, within the body of the function, we may use base-relative address-
ing against the base pointer to refer to both arguments and locals. The
arguments to the function follow the base pointer, so argument zero is at -
8(%rbp), argument one at -16(%rbp), and so forth. Past those are local vari-
ables to the function at -32(%rbp) and then saved registers at -48(%rbp).
The stack pointer points to the last item on the stack. If we use the stack
for additional purposes, data will be pushed to further negative values.
(Note that we have assumed all arguments and variables are 8 bytes large:
different types would result in different offsets.)


##### 10.3. X86 ASSEMBLY LANGUAGE 165

Here is a complete example that puts it all together. Suppose that you
have a B-minor function defined as follows:

compute: function integer
( a: integer, b: integer, c: integer ) =
{
x:integer = a+b+c;
y:integer = x*5;
return y;
}

A complete translation of the function is on the next page. The code
given is correct, but rather conservative. As it turned out, this particular
function didn’t need to use registers%rbxand%r15, so it wasn’t neces-
sary to save and restore them. In a similar way, we could have kept the
arguments in registers without saving them to the stack. The result could
have been computed directly into%raxrather than saving it to a local vari-
able. These optimizations are easy to make when writing code by hand,
but not so easy when writing a compiler.
For your first attempt at building a compiler, your code created will
(probably) not be very efficient if each statement is translated indepen-
dently. The preamble to a function must save all the registers, because it
does not knowa prioriwhich registers will be used later. Likewise, a state-
ment that computes a value must save it back to a local variable, because
it does not know beforehand whether the local will be used as a return
value. We will explore these issues later in Chapter 12 on optimization.


##### 166 CHAPTER 10. ASSEMBLY LANGUAGE

**Figure 10.6: Complete X86 Example**
.global compute
compute:
##################### preamble of function sets up stack
pushq %rbp # save the base pointer
movq %rsp, %rbp # set new base pointer to rsp

pushq %rdi # save first argument (a) on the stack
pushq %rsi # save second argument (b) on the stack
pushq %rdx # save third argument (c) on the stack

subq $16, %rsp # allocate two more local variables

pushq %rbx # save callee-saved registers
pushq %r12
pushq %r13
pushq %r14
pushq %r15

######################## body of function starts here
movq -8(%rbp), %rbx # load each arg into a register
movq -16(%rbp), %rcx
movq -24(%rbp), %rdx

addq %rdx, %rcx # add the args together
addq %rcx, %rbx
movq %rbx, -32(%rbp) # store the result into local 0 (x)

movq -32(%rbp), %rbx # load local 0 (x) into a register.
movq $5, %rcx # load 5 into a register
movq %rbx, %rax # move argument in rax
imulq %rcx # multiply them together
movq %rax, -40(%rbp) # store the result in local 1 (y)

movq -40(%rbp), %rax # move local 1 (y) into the result

#################### epilogue of function restores the stack
popq %r15 # restore callee-saved registers
popq %r14
popq %r13
popq %r12
popq %rbx

movq %rbp, %rsp # reset stack to base pointer.
popq %rbp # restore the old base pointer

ret # return to caller


##### 10.4. ARM ASSEMBLY 167

### 10.4 ARM Assembly

The ARM processor architecture has a history almost as long as the X86
architecture. It originated as the 32-bit **Acorn RISC Machine** used in the
**Acorn Archimedes** personal computer in 1987, and has since evolved into
a wide-used low-power CPU employed in many embedded and mobile
systems, now known as the **Advanced RISC Machine (ARM)**. The evolv-
ing architecture has been implemented by a number of chip vendors work-
ing from a common architecture definition. The most recent versions of
the architecture are ARMv7-A (32-bit) and ARMv8-A (64-bit.) This chap-
ter will focus on the 32-bit architecture, with some remarks on differences
in the 64-bit architecture at the end.
ARM is an example of a **Reduced Instruction Set Computer (RISC)**
rather than a **Complex Instruction Set Computer (CISC)**. Compared to
X86, ARM relies on a smaller set of instructions which are simpler to pipeline
or run in parallel, reducing the complexity and energy consumption of the
chip. ARM is sometimes considered “partially” RISC due to a few ex-
ceptions. For example, the difference in the time to execute some ARM
instructions makes the pipeline imperfect, the inclusion of a barrel shifter
for preprocessing brings forward more complex instructions, and condi-
tional execution decreases some of the potential instructions executed and
leads to less branching instructions so less energy used by the processor.
We will mainly be focusing on the elements of the instruction set which
allow us to do the most in a compiler, leaving the more complex aspects
and optimizations of the programming language for later.

#### 10.4.1 Registers and Data Types

ARM-32 has a set of 16 general purpose registers,r0-r15, with the follow-
ing conventions for use:

```
Name Alias Purpose
r0 General Purpose Register
r1 General Purpose Register
......
r10 General Purpose Register
r11 fp Frame Pointer
r12 ip Intra-Procedure-Call Scratch Register
r13 sp Stack Pointer
r14 lr Link Register (Return Address)
r15 pc Program Counter
```
In addition to general purpose registers, there are two registers that
cannot be directly accessed: the Current Program Status Register (CPSR)
and the Saved Program Status Register (SPSR). These hold the results of
comparison operations, as well as privileged data regarding the process


##### 168 CHAPTER 10. ASSEMBLY LANGUAGE

state. A user-level program cannot access these directly, but they can be
set as a side effect of some operations.
ARM uses the following suffixes to indicate data sizes. Note that these
havedifferentmeanings than in X86 assembly! If no suffix is given, the
assembler assumes an unsigned word operand. Signed types are used to
provide appropriate sign-extension when loading a small data type into a
larger register. There is no register naming structure for anything below a
word.

```
Data Type Suffix Size
Byte B 8 bits
Halfword H 16 bits
Word W 32 bits
Double Word - 64 bits
Signed Byte SB 8 bits
Signed Halfword SH 16 bits
Signed Word SW 32 bits
Double Word - 64 bits
```
#### 10.4.2 Addressing Modes

ARM makes the use of two different classes of instructions to move data
between registers and between registers and memory.MOVcopies data and
constants between registers, whileLDR(load) andSTR(store) instructions
are used to move data between registers and memory.
TheMOVinstruction is used to move a known immediate value into
a given register or move another register into the first register. In ARM,
immediate values are denoted by a#. However, these immediate values
must be 16-bits or less. If they are greater, theLDRinstruction must be
used instead. Most ARM instructions indicate the destination register on
the left and the source register on the right. (STRis an exception.) So for
moving data between immediate values and registers we would have the
following:

```
Mode Example
Immediate MOV r0, #3
Register MOV r1, r0
```
The mnemonic letter for each data type can be appended to theMOVin-
struction allowing us to be sure of which is being transferred and how that
data is being transferred. If not, the assembler assumes an entire word.
To move values in and out of memory, use the Load (LDR) and Store
(STR) instructions, both of which indicate the source or destination register
as the first argument, and the source or destination memory address as the
second argument. In the simplest case, the address is given by a register
and indicated in brackets:


##### 10.4. ARM ASSEMBLY 169

```
Figure 10.7: ARM Addressing Modes
```
```
Address Mode Example
Literal LDR Rd, =0xABCD1234
Absolute Address LDR Rd, =label
Register Indirect LDR Rd, [Ra]
Pre-indexing - Immediate LDR Rd, [Ra, #4]
Pre-indexing - Register LDR Rd, [Ra, Ro]
Pre-indexing - Immediate & Writeback LDR Rd, [Ra, #4]!
Pre-indexing - Register & Writeback LDR Rd, [Ra, Ro]!
Post-indexing - Immediate LDR Rd, [Ra], #4
Post-indexing - Register LDR Rd, [Ra], Ro
```
```
LDR Rd, [Ra]
STR Rs, [Ra]
```
In this case, Rd denotes the destination register, Rs denotes the source
register and Ra denotes the register containing the address. (Note that the
memory address must be aligned to the data type: a byte can load a value
from any address, a half-word from every even address, and so on.)
BothLDRandSTRsupport a variety of addressing modes, shown in
Figure 10.7. First,LDRcan be used to load a literal value (or label address)
of a full 32-bits into a register. (See the next section for a full explanation
of this.) Unlike X86, there is no single instruction that loads a value from a
given memory address. To accomplish this, you must first load the address
into a register, and then perform a register-indirect load:

LDR r1, =x
LDR r2, [r1]

A number of more complex addressing modes are available which fa-
cilitate the implementation of pointers, arrays, and structures in high level
programs. Pre-indexing modes add a constant (or register) to a base regis-
ter, and then load from the computed address:

LDR r1, [r2, #4] ; Load from address r2 + 4
LDR r1, [r2, r3] ; Load from address r2 + r3

It is also possible to write-back to the base register by appending a bang
(!) character. This indicates that the computed address should be saved in
the base register after the address is loaded:

LDR r1, [r2, #4]! ; Load from r2 + 4 then r2 += 4
LDR r1, [r2, r3]! ; Load from r2 + r3 then r2 += r3


##### 170 CHAPTER 10. ASSEMBLY LANGUAGE

Post-indexing modes do the same thing, but in the reverse order. First,
the load is performed from the base register, then the base register is incre-
mented:

LDR r1, [r2], #4 ; Load from r2 then r2 += 4
LDR r1, [r2], r3 ; Load from r2 then r2 += r3

These complex pre-indexing and post-indexing modes make it possi-
ble to have single-instruction implementations of idiomatic C operations
likeb = a++. The corresponding modes are also available to theSTRin-
struction.
Absolute addresses (and other large literals) are somewhat more com-
plicated in ARM. Because every instruction must fit into a 32-bit word,
it is not possible to fit a 32-bit address into an instruction, alongside the
opcode. Instead, large literals must be stored in a **literal pool** , which is
a small region of data inside the code section of the program. A literal
can be loaded from a pool with a PC-relative load instruction, which can
reference± 4096 bytes from the loading instruction. This results in several
small literal pools being scattered throughout the program, so that each
one is close to the load instruction that uses it.
The ARM assembler generally hides this complexity from the user.
When a label or large literal is prefixed with an equals sign, this indicates
to the assembler that the marked value should be placed into a literal pool,
and a corresponding PC-relative instruction emitted instead.
For example, the following instructions, which load the address ofx
intor1, then the value ofxintor2:

LDR r1, =x
LDR r2, [r1]

Will be expanded into this load of the address ofxfrom an adjacent
literal pool, followed by loading the value ofx:

LDR r1, .L1
LDR r2, [r1]
B .end
.L1:
.word x
.end:

#### 10.4.3 Basic Arithmetic

ARM provides three-address arithmetic instructions on registers. TheADD
andSUBinstructions specify the result register as the first argument, and
compute on the second and third arguments. The third operand may be
an 8-bit constant, or a register with an optional shift applied. The variants


##### 10.4. ARM ASSEMBLY 171

with carry-in enabled will add the C bit of the CPSR to the result. All
four take an optional S suffix which will set the condition flags (including
carry) on completion.

```
Instruction Example
Add ADD Rd, Rm, Rn
Add with carry-in ADC Rd, Rm, Rn
Subtract SUB Rd, Rm, Rn
Subtract with carry-in SBC Rd, Rm, Rn
```
Multiplication works much the same way, except that multiplying two
32-bit numbers could result in a 64-bit value. The ordinaryMULdiscards
the high bits of the results, whileUMULLputs the 64-bit result in two 32-bit
registers. The signed variantSMULLwill sign extend the high register as
needed.

```
Instruction Example
Multiplication MUL Rd, Rm, Rn
Unsigned Long Multiplication UMULL RdHi, RdLo, Rm, Rn
Signed Long Multiplication SMULL RdHi, RdLo, Rm, Rn
```
There is no division instruction in ARM, because it cannot be carried
out in a single pipelined cycle. Instead, when division is needed, it is
accomplished by invoking an external function in a standard library. This
is left as an exercise for the reader.
The logical instructions are very similar in structure to the arithmetic
instructions. We have the bitwise-and, bitwise-or, bitwise-exclusive-or
and bitwise-bit-clear, which is the equivalent of a bitwise-and of the first
value and the inverted second value. The move-notMVNinstruction per-
forms a bitwise-not while moving from one register to another.

```
Instruction Example
Bitwise-And AND Rd, Rm, Rn
Bitwise-Or ORR Rd, Rm, Rn
Bitwise-Xor EOR Rd, Rm, Rn
Bitwise-Bit-Clear BIC Rd, RM, Rn
Move-Not MVN Rd, Rn
```
**10.4.4 Comparisons and Branches**

TheCMPinstruction compares two values and sets the N (negative) and
Z (zero) flags in the CPSR, to be read by later instructions. In the case of
comparing a register and an immediate value, the immediate must be the
second operand:

```
CMP Rd, Rn
CMP Rd, #imm
```

##### 172 CHAPTER 10. ASSEMBLY LANGUAGE

```
Figure 10.8: ARM Branch Instructions
```
```
Opcode Meaning
B Branch Always BL Branch and Link
BX Branch and Exchange BLX Branch-Link-Exchange
BEQ Equal BVS Overflow Set
BNE Not Equal BVC Overflow Clear
BGT Greater Than BHI Higher (unsigned>)
BGE Greater Than or Equal BHS Higher or Same (uns.>=)
BLT Less Than BLO Lower (unsigned<)
BLE Less Than or Equal BLS Lower or Same (uns.<=)
BMI Negative BPL Positive or Zero
```
In addition, an ”S” can be appended to the arithmetic instructions to
update the CPSR in a similar way. For example,SUBSwill subtract two
values, store the result, and update the CPSR.
A variety of branch instructions consult the previously-set values of
the CPSR, and then jump to a given label, if the appropriate flags are set.
An unconditional branch is specified with simplyB.
For example, to count from zero to five:

MOV r0, #0
loop: ADD r0, r0, 1
CMP r0, #5
BLT loop

And to conditionally assign a global variable y ten if x is greater than 0
and 20 if it is not

LDR r0, =x
LDR r0, [r0]
CMP r0, #0
BGT .L1
.L0:
MOV r0, #20
B .L2
.L1:
MOV r0, #10
.L2:
LDR r1, =y
STR r0, [r1]

The branch-and-link (BL) instruction, is used to implement function
calls.BLsets the link register to be the address of the next instruction, and
then jumps to the given label. The link register is then used as the return


##### 10.4. ARM ASSEMBLY 173

address at the conclusion of the function. TheBXinstruction branches to
the address given in a register, and is most frequently used to return from a
function call by branching to the link register.BLXperforms a branch-and-
link to the address given by a register, and can be used to invoke function
pointers, virtual methods, or any other indirect jump.
A special feature of the ARM instruction set is **conditional execution**.
A 4-bit field in each instruction word indicates one of 16 possible condi-
tions that must hold, otherwise the instruction is ignored. The various
types of conditional branch shown above are simply a plain branch (B) in-
struction with the various conditions applied. The same two letter suffixes
can be applied to almost any instruction.
For example, suppose we have the following code fragment, which
increments eitheraorb, depending on which one is smaller:

if(a<b) { a++; } else { b++; }

Instead of implementing this as control flow using branches and labels,
we can simply make each of the two additions conditional upon a previous
comparison. Whichever condition holds true will be executed, and the
other skipped. Assuming thataandbare held inr0andr1respectively:

CMP r0, r1
ADDLT r0, r0, #1
ADDGE r1, r1, #1

#### 10.4.5 The Stack

The stack is an auxiliary data structure used primarily to record the func-
tion call history of the program along with local variables that do not fit
in registers. By convention, the stack growsdownwardfrom high values to
low values. Thespregister is known as the **stack pointer** and keeps track
of the bottom-most item on the stack.
To push ther0register onto the stack, we must subtract the size of the
register fromsp, and then storer0into the location pointed to bysp:

SUB sp, sp, #4
STR r0, [sp]

Alternatively, this can be done with a single instruction making use of
pre-indexing and writeback:

STR r0, [sp, #-4]!

ThePUSHpseudo-instruction accomplishes the same thing, but can
also move any number of registers (encoded as a bitmask) to the stack.
Curly braces are used to indicate the list of registers to push:


##### 174 CHAPTER 10. ASSEMBLY LANGUAGE

```
Figure 10.9: Summary of ARM Calling Convention
```
- The first four arguments are placed in registers r0, r1, r2 and r3.
- Additional arguments are pushed on the stack in reverse order.
- The caller must save r0-r3 and r12, if needed.
- the caller must always save r14, the link register.
- The **callee** must save r4-r11, if needed.
- The result is placed in r0.

PUSH {r0,r1,r2}

```
Popping a value off the stack involves the opposite:
```
LDR r0, [sp]
ADD sp, sp, #4

```
Once again this can be done with a single instruction:
```
LDR r0, [sp], #4

```
And, to pop a set of registers all at once:
```
POP {r0,r1,r2}

Unlike X86, any data items ranging from a byte to a double-word can
be pushed on to the stack, as long as data alignment is respected.

#### 10.4.6 Calling a Function

ARM uses a register calling convention described by the ARM-Thumb
Procedure Call Standard (ATPCS)[4], which is summarized in Figure 10.9.
To call a function, place the desired arguments in the registersr0-r3,
save the (current) value of the link register, and then use theBLinstruction
to jump to the function. Upon return, restore the previous value of the link
register, and examine the result in registerr0.
For example, the following C function:

int x=0;
int y=10;
int main() {
x = printf("value: %d\n",y);
}


##### 10.4. ARM ASSEMBLY 175

```
Figure 10.10: ARM Register Assignments
```
```
Register Purpose Who Saves?
r0 argument 0 / result not saved
r1 argument 1 CALLER saves
r2 argument 2 CALLER saves
r3 argument 3 CALLER saves
r4 scratch callee saves
```
```
r10 scratch callee saves
r11 frame pointer callee saves
r12 intraprocedure CALLER saves
r13 stack pointer callee saves
r14 link register CALLER saves
r15 program counter saved in link register
```
```
Would become the following in ARM:
```
.data
x: .word 0
y: .word 10
S0: .ascii "value: %d\012\000"
.text
main:
LDR r0, =S0 @ Load address of S0
LDR r1, =y @ Load address of y
LDR r1, [r1] @ Load value of y

```
PUSH {ip,lr} @ Save registers
BL printf @ Call printf
POP {ip,lr} @ Restore registers
```
LDR r1, =x @ Load address of x
STR r0, [r1] @ Store return value in x
.end

#### 10.4.7 Defining a Leaf Function

Because function arguments are passed in registers, it is easy to write a
**leaf function** that computes a value without calling any other functions.
For example, code for the following function:

square: function integer ( x: integer ) =
{
return x*x;


##### 176 CHAPTER 10. ASSEMBLY LANGUAGE

##### }

```
Could be as simple as this:
```
.global square
square:
MUL r0, r0, r0 @ multiply argument by itself
BX lr @ return to caller

Unfortunately, this won’t work for a function that wants to invoke
other functions, because we haven’t set up the stack properly. A more
complex approach is needed for the general case.

#### 10.4.8 Defining a Complex Function

A complex function must be able to invoke other functions and compute
expressions of arbitrary complexity, and then return to the caller with the
original state intact. Consider the following recipe for a function that ac-
cepts three arguments and uses two local variables:

func:
PUSH {fp} @ save the frame pointer
MOV fp, sp @ set the new frame pointer
PUSH {r0,r1,r2} @ save the arguments on the stack
SUB sp, sp, #8 @ allocate two more local variables
PUSH {r4-r10} @ save callee-saved registers

```
@@@ body of function goes here @@@
```
```
POP {r4-r10} @ restore callee saved registers
MOV sp, fp @ reset stack pointer
POP {fp} @ recover previous frame pointer
BX lr @ return to the caller
```
Through this method, we ensure that we are able to save all the values
in the registers into the stack and ensure that no data will be lost. The
stack, once this has been done, looks very similar to that of the X86 stack,
just with some extra callee-saved registers stored on the stack.
Here is a complete example that puts it all together. Suppose that you
have a B-minor function defined as follows:

compute: function integer
( a: integer, b: integer, c: integer ) =
{
x: integer = a+b+c;
y: integer = x*5;
return y;
}


##### 10.4. ARM ASSEMBLY 177

```
Figure 10.11: Example ARM Stack Frame
```
```
Contents Address
Saved r12 [fp, #8]
Old LR [fp, #4]
Old Frame Pointer [fp] ←fp points here
Argument 2 [fp, #-4]
Argument 1 [fp, #-8]
Argument 0 [fp, #-12]
Local Variable 1 [fp, #-16]
Local Variable 0 [fp, #-20]
Saved r10 [fp, #-24]
Saved r9 [fp, #-28]
Saved r8 [fp, #-32]
Saved r7 [fp, #-36]
Saved r6 [fp, #-40]
Saved r5 [fp, #-44]
Saved r4 [fp, #-48] ←sp points here
```
A complete translation of the function is on the next page. Note that
this is one of many ways to construct a valid stack frame for a function
definition. Other approaches are valid, as long as the function uses the
stack frame consistently. For example, the callee could first push all of the
argument and scratch registers on the stack, and then allocate space for
local variables below that. (The reverse process must be used on function
exit, of course.)
Another common approach is for the calleePUSH {fp,ip,lr,pc}
on to the stack, before pushing arguments and local variables. While not
strictly required for implementing the function, it provides additional de-
bugging information in the form of a **stack backtrace** so that a debugger
can look backwards through the call stack and easily reconstruct the cur-
rent execution state of the program.
The code given is correct, but rather conservative. As it turned out,
this particular function didn’t need to use registersr4andr5, so it wasn’t
necessary to save and restore them. In a similar way, we could have kept
the arguments in registers without saving them to the stack. The result
could have been computed directly intor0rather than saving it to a lo-
cal variable. These optimizations are easy to make when writing code by
hand, but not so easy when writing a compiler.
For your first attempt at building a compiler, your code created will
(probably) not be very efficient if each statement is translated indepen-
dently. The preamble to a function must save all the registers, because it
does not knowa prioriwhich registers will be used later. Likewise, a state-
ment that computes a value must save it back to a local variable, because


##### 178 CHAPTER 10. ASSEMBLY LANGUAGE

```
Figure 10.12: Complete ARM Example
```
.global compute
compute:
@@@@@@@@@@@@@@@@@@ preamble of function sets up stack
PUSH {fp} @ save the frame pointer
MOV fp, sp @ set the new frame pointer
PUSH {r0,r1,r2} @ save the arguments on the stack
SUB sp, sp, #8 @ allocate two more local variables
PUSH {r4-r10} @ save callee-saved registers

@@@@@@@@@@@@@@@@@@@@@@@@ body of function starts here
LDR r0, [fp,#-12] @ load argument 0 (a) into r0
LDR r1, [fp,#-8] @ load argument 1 (b) into r1
LDR r2, [fp,#-4] @ load argument 2 (c) into r2
ADD r1, r1, r2 @ add the args together
ADD r0, r0, r1
STR r0, [fp,#-20] @ store the result into local 0 (x)
LDR r0, [fp,#-20] @ load local 0 (x) into a register.
MOV r1, #5 @ move 5 into a register
MUL r2, r0, r1 @ multiply both into r2
STR r2, [fp,#-16] @ store the result in local 1 (y)
LDR r0, [fp,#-16] @ move local 1 (y) into the result

@@@@@@@@@@@@@@@@@@@ epilogue of function restores the stack
POP {r4-r10} @ restore callee saved registers
MOV sp, fp @ reset stack pointer
POP {fp} @ recover previous frame pointer
BX lr @ return to the caller


##### 10.4. ARM ASSEMBLY 179

it does not know beforehand whether the local will be used as a return
value. We will explore these issues later in Chapter 12 on optimization.

#### 10.4.9 64-bit Differences

The 64-bit ARMv8-A architecture provides two execution modes: The A32
mode supports the 32-bit instruction set described above, and the A64
mode supports a new 64-bit execution model. This permits a 64-bit CPU
with a supporting operating system to execute a mix of 32-bit and 64-bit
programs simultaneously. Though not binary compatible with A32, the
A64 model follows much of the same architectural principles, with a few
key changes:
**Word Size.** A64 instructions are still a fixed size of 32 bits, however,
registers and address computations are 64 bits.
**Registers.** A64 has 32 64-bit registers, namedx0-x31. x0is a dedi-
cated zero register: when read, it always provides the value zero, when
written, there is no effect.x1-x15are general purpose registers,x16and
x17are for interprocess communication,x29is the frame pointer,x30is
the link register andx31is the stack pointer. (The program counter is not
directly accessible from user code.) Instead of using a data type suffix, a
32-bit value may be indicated by naming a register asw#.
**Instructions.** A64 instructions are largely the same as A32, using the
same mnemonics, with a few differences. Conditional predicates are no
longer part of every instruction. Instead, all conditional codes must per-
form an explicitCMPand then a conditional branch. TheLDM/STMin-
structions and pseudo-instructionsPUSH/POPare not available and must
be replaced with a sequence of explicit loads and stores. (This can be made
more efficient by usingLDP/STPwhich load and store pairs of registers.
**Calling Convention.** To invoke a function, the first eight arguments
are placed in registersx0-x7, and the remainder are pushed on to the
stack. The caller must preserve registersx9-x15andx30while the callee
must preservex19-x29. The (scalar) return value is placed inx0, while
extended return values are pointed to byx8.


##### 180 CHAPTER 10. ASSEMBLY LANGUAGE

### 10.5 Further Reading

This chapter has given you a brief orientation to the core features of the
X86 and ARM architectures, enough to write simple programs in the most
direct way. However, you will certainly need to look up specific details
of individual instructions in order to better understand their options and
limitations. Now you are ready to read the detailed reference manuals and
keep them handy while you construct your compiler:

1. Intel64 and IA-32 Architectures Software Developer Manuals. Intel
    Corp., 2017.
    [http://www.intel.com/content/www/us/en/processors/](http://www.intel.com/content/www/us/en/processors/)
    architectures-software-developer-manuals.html
2. System V Application Binary Interface, Jan Hubicka, An-
    dreas Jaeger, Michael Matz, and Mark Mitchell (editors), 2013.
    https://software.intel.com/sites/default/files/article/
    402129/mpx-linux64-abi.pdf
3. ARM Architecture Reference Manual ARMv8. ARM Limited, 2017.
    https://static.docs.arm.com/ddi0487/bb/DDI0487B_b_
    armv8_arm.pdf.
4. The ARM-THUMB Procedure Call Standard. ARM Limited, 2000.
    https://developer.arm.com/documentation/ihi0042/f/


##### 181

## Chapter 11 – Code Generation

### 11.1 Introduction

Congratulations, you have made it to the final stage of the compiler! After
scanning and parsing the source code, constructing the AST, performing
type checking, and generating an intermediate representation, we are now
ready to generate some code.
To start, we are going to take a na ̈ıve approach to code generation, in
which we consider each element of the program in isolation. Each ex-
pression and statement will be generated as a standalone unit, without
reference to its neighbors. This is easy and straightforward, but it is con-
servative and will lead to a large amount of non-optimal code. But it will
work, and give you a starting point for thinking about more sophisticated
techniques.
The examples will focus on X86-64 assembly code, but they are not
hard to adapt to ARM and other assembly languages as needed. As with
previous stages, we will define a method for each element of a program.
declcodegenwill generate code for a declaration, callingstmtcodegen
for a statement,exprcodegenfor an expression, and so on. These rela-
tionships are shown in Figure 11.1.
Once you have learned this basic approach to code generation, you
will be ready for thefollowingchapter, in which we consider more complex
methods for generating more highly optimized code.

### 11.2 Supporting Functions

Before generating some code, we need to set up a few supporting functions
to keep track of a few details. To generate expressions, you will need some
**scratch registers** to hold intermediate values between operators. Write
three functions with the following interface:

int scratch_alloc();
void scratch_free( int r );
const char * scratch_name( int r );

Looking back at Chapter 10, you can see that we set aside each regis-
ter for a purpose: some are for function arguments, some for stack frame


##### 182 CHAPTER 11. CODE GENERATION

```
Figure 11.1: Code Generation Functions
```
```
decl_codegen
```
```
stmt_codegen
```
```
scratch_free
```
```
expr_codegen label_create label_print
```
```
symbol_codegen scratch_alloc
```
management, and some are available for scratch values. Take those scratch
registers and put them into a table like this:

```
r 0 1 2 3 4 5 6
name %rbx %r10 %r11 %r12 %r13 %r14 %r15
inuse X X
```
Then, writescratchallocto find an unused register in the table,
mark it as in use, and return the register numberr.scratchfreeshould
mark the indicated register as available.scratchnameshould return the
name of a register, given its numberr. Running out of scratch registers is
possible, but unlikely, as we will see below. For now, ifscratchalloc
cannot find a free register, just emit an error message and halt.
Next, we will need to generate a large number of unique, anonymous
labels that indicate the targets of jumps and conditional branches. Write
two functions to generate and display labels:

int label_create();
const char * label_name( int label );

labelcreatesimply increments a global counter and returns the
current value.labelnamereturns that label in a string form, so that label
15 is represented as".L15".


##### 11.3. GENERATING EXPRESSIONS 183

Finally, we need a way of mapping from the symbols in a program to
the assembly language code representing those symbols. For this, write a
function to generate the address of a symbol:

const char * symbol_codegen( struct symbol *s );

This function returns a string which is a fragment of an instruction,
representing the address computation needed for a given symbol. Write
symbolcodegento first examine the scope of the symbol. Global vari-
ables are easy: the name in assembly language is the same as in the source
language. If you have asymbolstructure representing the global variable
count:integer, thensymbolcodegenshould simply returncount.
Symbols that represent local variables and function parameters should
instead return an address computation that yields the position of that local
variable or parameter on the stack. The groundwork for this was laid in
the typechecking phase, where you assigned each symbol a unique num-
ber, starting with the parameters and continuing with each local variable.
For example, suppose you have this function definition:

f: function void ( x: integer, y: integer ) =
{
z: integer = 10;
return x + y + z;
}

In this case,xwas assigned a position of zero,yis position one, and
zis position two. Now look back at Figure 10.5, which shows the stack
layout on the X86-64 processor. Position zero is at the address-8(%rbp),
position one is at-16(%rbp), and position two is at-24(%rbp).
Given that, you can now extendsymbolcodegento return a string
describing the precise stack address of local variables and parameters,
knowing only its position in the stack frame.

### 11.3 Generating Expressions

The basic approach to generating assembly code for an expression is to
perform a post-order traversal of the AST or DAG, and emit one or more
instructions for each node. The main idea is to keep track of the registers
in which each intermediate value is stored. To do this, add aregfield to
the AST or DAG node structure, which will hold the number of a register
returned byscratchalloc. As you visit each node, emit an instruction
and place into theregfield the number of the register containing that
value. When the node is no longer needed, callscratchfreeto release
it.


##### 184 CHAPTER 11. CODE GENERATION

Suppose we want to generate X86 code for the following DAG, where
a,bandcare global integers:

```
ASSIGN
```
```
ISUB c
```
```
IADD b
```
```
a 3
```
1. MOVQ a, R0
2. MOVQ $3, R1
3. ADDQ R0, R1
4. MOVQ b, R0
5. SUBQ R0, R1
6. MOVQ R1, c

```
Figure 11.2: Example of Generating X86 Code from a DAG
```
```
A post-order traversal would visit the nodes in the following order:
```
1. Visit theanode. Callscratchallocto allocate a new register (0)
    and save that innode->reg. Then emit the instructionMOVQ a, R0
    to load the value into register zero.^1
2. Visit the 3 node. Callscratchallocto allocate a new register (1)
    and emit the instructionMOVQ $3, R1.
3. Visit theIADDnode. By examining the two children of this node,
    we can see that their values are stored in registers R0 and R1, so we
    emit an instruction that adds them together:ADDQ R0, R1. This is
    a destructive two-address instruction which leaves the result in R1.
    R0 is no longer needed, so we callscratchfree(0).
4. Visit thebnode. Callscratchallocto allocate a new register (0)
    and emitMOVQ b, R0.
5. Visit theISUBnode. EmitSUBQ R0, R1, leaving the result in R1,
    and freeing register R0.
6. Visit thecnode, but don’t emit anything, because it is the target of
    the assignment.
7. Visit theASSIGNnode and emitMOVQ R1, c.

(^1) Note that the actual name of register R0 isscratchname(0), which is%rbx. To keep
the example clear, we will call them R0, R1, etc. for now.


##### 11.3. GENERATING EXPRESSIONS 185

And here is the same code with the actual register names provided by
scratchname:

MOVQ a, %rbx
MOVQ $3, %r10
ADDQ %r10, %rbx
MOVQ b, %rbx
SUBQ %rbx, %r10
MOVQ %r10, c

Here is how to implement it in code. Write a function calledexprcodegen
that first recursively callsexprcodegenfor its left and right children.
This will cause each child to generate code such that the result will be left
in the register number noted in theregisterfield. The current node then
generates code using those registers, and frees the registers it no longer
needs. Figure 11.3 gives a skeleton for this approach.
A few additional refinements are needed to the basic process.
First, not all symbols are simple global variables. When a symbol forms
part of an instruction, usesymbolcodegento return the string that gives
the specific address for that symbol. For example, ifawas the first param-
eter to the function, then the first instruction in the sequence would have
looked like this instead:

MOVQ -8(%rbp), %rbx

Second, some nodes in the DAG may require multiple instructions, so
as to handle peculiarities of the instruction set. You will recall that the X86
IMULonly takes one argument, because the first argument is always%rax
and the result is always placed in%raxwith the overflow in%rdx. To
perform the multiply, we must move one child register into%rax, multi-
ply by the other child register, and then move the result from%raxto the
destination scratch register. For example, the expression(x*10)would
translate as this:

MOV $10, %rbx
MOV x, %r10
MOV %rbx, %rax
IMUL %r10
MOV %rax, %r11

Of course, this also means that%raxand%rdxcannot be used for other
purposes while a multiply is in progress. Since we have a large number
of scratch registers to work with, we will just not use%rdxfor any other
purpose in our basic code generator.


##### 186 CHAPTER 11. CODE GENERATION

void expr_codegen( struct expr *e )
{
if(!e) return;

```
switch(e->kind) {
```
```
// Leaf node: allocate register and load value.
```
```
case EXPR_NAME:
e->reg = scratch_alloc();
printf("MOVQ %s, %s\n",
symbol_codegen(e->symbol),
scratch_name(e->reg));
break;
```
```
// Interior node: generate children, then add them.
```
```
case EXPR_ADD:
expr_codegen(e->left);
expr_codegen(e->right);
printf("ADDQ %s, %s\n",
scratch_name(e->left->reg),
scratch_name(e->right->reg));
e->reg = e->right->reg;
scratch_free(e->left->reg);
break;
```
```
case EXPR_ASSIGN:
expr_codegen(e->left);
printf("MOVQ %s, %s\n",
scratch_name(e->left->reg),
symbol_codegen(e->right->symbol));
e->reg = e->left->reg;
break;
```
...
}
}

```
Figure 11.3: Expression Generation Skeleton
```

##### 11.3. GENERATING EXPRESSIONS 187

```
ARG
```
```
IADD (null)
```
```
ASSIGN
```
```
CALL a
```
```
f ARG
```
```
10
```
```
b c
```
```
MOVQ $10, %rbx
MOVQ b, %r10
MOVQ c, %r11
ADDQ %r10, %r11
MOVQ %r11, %rsi
MOVQ %rbx, %rdi
PUSHQ %r10
PUSHQ %r11
CALL f
POPQ %r11
POPQ %r10
MOVQ %rax, %rbx
MOVQ %rbx, a
```
```
Figure 11.4: Generating Code for a Function Call
```
Third, how do we invoke a function? Recall that a function is repre-
sented by a single CALL node, with each of the arguments in an unbal-
anced tree of ARG nodes. Figure 11.4 gives an example of a DAG repre-
senting the expressiona=f(10,b+c).
The code generator must evaluate each of the ARG nodes, computing
the value of each left hand child. If the machine has a stack calling con-
vention, then each ARG node corresponds to a PUSH onto the stack. If the
machine has a register calling convention, generate all of the arguments,
then copy each one into the appropriate argument register after all argu-
ments are generated. Then, emit CALL on the function name, after saving
any caller-saved registers. When the function call returns, move the value
from the return register (%rax) into a newly allocated scratch register and
restore the caller-saved registers.
Finally, note carefully the side effects of expressions. Every expression
has a **value** which is computed and left in a scratch register for its parent
node to consider. Some expressions also have **side effects** which are ac-
tions in addition to the value. With some operators, it is easy to overlook
one or the other!
For example, the expression(x=10)yields avalueof 10, which means
you can use that expression anywhere a value is expected. This is what
allows you to writey=x=10orf(x=10). The expression also has theside
effectof storing the value 10 into the variable x. When you generate code
forx=10assignment, be sure to carry out the side effect of storing 10 into
x (that’s obvious) but also retain the value 10 in the register that represents
the value of the expression.


##### 188 CHAPTER 11. CODE GENERATION

### 11.4 Generating Statements

Now that we have encapsulated expression generation into a single func-
tionexprcodegen, we can begin to build up larger code structures that
rely upon expressions.stmtcodegenwill create code for all control flow
statements. Begin by writing a skeleton forstmtcodegenlike this:

void stmt_codegen( struct stmt *s )
{
if(!s) return;

switch(s->kind) {
case STMT_EXPR:
...
break;
case STMT_DECL:
...
break;
...
}
stmt_codegen(s->next);
}

```
Figure 11.5: Statement Generator Skeleton
```
Now consider each kind of statement in turn, starting with the easy
cases. If the statement describes a declarationSTMTDECLof a local vari-
able, then just delegate that by callingdeclcodegen:

case STMT_DECL:
decl_codegen(s->decl);
break;

A statement that consists of an expression (STMTEXPR) simply requires
that we callexprcodegenon that expression, and then free the scratch
register containing the top-level value of the expression. (In fact, every
timeexprcodegenis called, a scratch register should be freed.)

case STMT_EXPR:
expr_codegen(s->expr);
scratch_free(s->expr->reg);
break;

Areturnstatement must evaluate an expression, move it into the des-
ignated register for return values%rax, and then jump to the function epi-
logue, which will unwind the stack and return to the call point. (See below
for more details about the prologue.)


##### 11.4. GENERATING STATEMENTS 189

case STMT_RETURN:
expr_codegen(s->expr);
printf("MOV %s, %%rax\n",scratch_name(s->expr->reg));
printf("JMP .%s_epilogue\n",function_name);
scratch_free(s->expr->reg);
break;

(The careful reader will notice that this code needs to know the name
of the function that contains this statement. You will have to figure out a
way to pass that information down.)
Control flow statements are more interesting. It’s useful to first con-
sider what we want the output assembly language to look like, and then
work backwards to get the code for generating it.
Here is a template for a conditional statement:

**if (** expr **)** {

true-statements
} **else** {

```
false-statements
```
}

To express this in assembly, we must evaluate the control expression so
that its value resides in a known register. ACMPexpression is then used
to test if the value is equal to zero (false). If the expression is false, then
we must jump to the false branch of the statement with aJE(jump-if-
equal) statement. Otherwise, we continue through to the true branch of
the statement. At the end of the true statement, we mustJMPover the else
body to the end of the statement.

expr
CMP $0,register
JEfalse-label
true-statements
JMPdone-label
false-label **:**

```
false-statements
```
done-label **:**


##### 190 CHAPTER 11. CODE GENERATION

Once you have a skeleton of the desired code, writing the code gen-
erator is easy. First, generate two new labels, then callexprcodegen
for each expression,stmtcodegenfor each statement, and substitute the
few additional instructions as needed to make the overall structure.

case STMT_IF:
int else_label = label_create();
int done_label = label_create();
expr_codegen(s->expr);
printf("CMP $0, %s\n",scratch_name(s->expr->reg));
scratch_free(s->expr->reg);
printf("JE %s\n",label_name(else_label));
stmt_codegen(s->body);
printf("JMP %s\n",label_name(done_label));
printf("%s:\n",label_name(else_label));
stmt_codegen(s->else_body);
printf("%s:\n",label_name(done_label));
break;

A similar approach can be used to generate loops. Here is the source
template of a for-loop:

**for (** init-expr **;** expr **;** next-expr **)** {

```
body-statements
```
}

And here is the corresponding assembly template. First, evaluate the ini-
tializing expression. Then, upon each iteration of the loop, evaluate the
control expression. If false, jump to the end of the loop. If not, execute the
body of the loop, then evaluate the next expression.

```
init-expr
```
top-label **:**
expr
CMP $0,register
JE done-label
body-statements
next-expression
JMPtop-label
done-label **:**

Writing the code generator is left as an exercise for the reader. Just keep
in mind that each of the three expressions in a for-loop can be omitted. If


##### 11.4. GENERATING STATEMENTS 191

theinit-expror thenext-exprare omitted, they have no effect. If theexpris
omitted, it is assumed to be true.^2
Many languages have loop-control constructs likecontinue;and
break;. In these cases, the compiler must keep track of the labels as-
sociated with the current loop being generated, and convert these into a
JMPto the top label, or the done-label respectively.
Theprintstatement in B-Minor is a special case of an imperative
statement with variant behavior depending on the type of the expression
to be printed. For example, the followingprintstatement must generate
slightly different code for the integer, boolean, and string to be printed:

i: integer = 10;
b: boolean = true;
s: string = "\n";
print i, b, s;

Obviously, there is no simple assembly code corresponding to the dis-
play of an integer. In this case, a common approach is to reduce the
task into an abstraction that we already know. The printing of integers,
booleans, strings, etc., can be delegated to function calls that explicitly
perform those actions. The generated code forprint i, b, sis then
equivalent to this:

print_integer(i);
print_boolean(b);
print_string(s);

So, to generate aprintstatement, we simply generate the code for
each expression to be printed, determine the type of the expression with
exprtypecheckand then emit the corresponding function call.
Of course, each of these functions must be written and then linked into
each instance of a B-Minor program, so they are available when needed.
These functions, and any others necessary, are collectively known as the
**runtime library** for B-Minor programs. As a general rule, the more high-
level a programming language, the more runtime support is needed.

(^2) Yes, each of the three components of a for-loop are expressions. It is customary that the
first has a side effect of initialization (i=0), the second is a comparison (i<10), and the third
has a side effect to generate the next value (i++), but they are all just plain expressions.


##### 192 CHAPTER 11. CODE GENERATION

### 11.5 Conditional Expressions

Now that you know how to generate control flow statements, we must
return to one aspect of expression generation. Conditional expressions
(less-than, greater-than, equals, etc.) compare two values and return a
boolean value. They most frequently appear in control flow expressions
but can also be used as simple values, like this:

b: boolean = x < y;

The problem is that there is no single instruction that simply performs
the comparison and places the boolean result in a register. Instead, you
must go the long way around and make a control flow construct that com-
pares the two expressions, then constructs the desired result.
For example, if you have a conditional expression like this:

```
left-expr < right-expr
```
then generate code according to this template:

left-expr
right-expr
CMPleft-register,right-register
JLTtrue-label
MOVfalse, result-register
JMPdone-label
true-label:
MOVtrue, result-register
done-label:

Of course, for different conditional operators, use a different jump in-
struction in the appropriate place. With slight variations, you can use the
same approach to implement the ternary conditional operator(x?a:b)
found in many languages.
A funny outcome of this approach is that if you generate code for an
if-statement likeif(x>y){...}in the obvious way, you will end up with
twoconditional structures in the assembly code. The first conditional com-
putes the result ofx>yand places that in a register. The second condi-
tional compares that results against zero and then jumps to the true or false
branch of the statement. With a little careful coding, you can check for this
common case and generate a single conditional statement that evaluates
the expression and uses one conditional jump to implement the statement.


##### 11.6. GENERATING DECLARATIONS 193

### 11.6 Generating Declarations

Finally, emitting the entire program is a matter of traversing each declara-
tion of code or data and emitting its basic structure. Declarations can fall
into three cases: global variable declarations, local variable declarations,
and global function declarations. (B-Minor does not permit local function
declarations.)
Global data declarations are simply a matter of emitting a label along
with a suitable directive that reserves the necessary space, and an initial-
izer, if needed. For example, these B-Minor declarations at global scope:

i: integer = 10;
s: string = "hello";
b: array [4] boolean = {true, false, true, false};

```
Should yield these output directives:
```
.data
i: .quad 10
s: .string "hello"
b: .quad 1, 0, 1, 0

Note that a global variable declaration can only be initialized by a con-
stant value (and not a general expression) precisely because the data sec-
tion of the program can only contain constants (and not code). If the pro-
grammer accidentally put code into the initializer, then the typechecker
should have discovered that and raised an error before code generation
began.
Emitting a local variable declaration is much easier. (This only hap-
pens whendeclcodegenis called bystmtcodegeninside of a func-
tion declaration.) Here, you can assume that space for the local variable
has already been established by the function prologue, so no stack manip-
ulations are necessary. However, if the variable declaration has an initial-
izing expression (x:integer=y*10;) then you must generate code for
the expression, store it in the local variable, and free the register.
Function declarations are the final piece. To generate a function, you
must emit a label with the function’s name, followed by the function pro-
logue. The prologue must take into account the number of parameters
and local variables, making the appropriate amount of space on the stack.
Next comes the body of the function, followed by the function epilogue.
The epilogue should have a unique label so thatreturnstatements can
easily jump there.


##### 194 CHAPTER 11. CODE GENERATION

### 11.7 Exercises

1. Write a legal expression that would exhaust the six available scratch
    registers, if using the technique described in this chapter. In general,
    how many registers are needed to generate code for an arbitrary ex-
    pression tree?
2. When using a register calling convention, why is it necessary to gen-
    erate values forallthe function arguments before moving the values
    into the argument registers?
3. Can a global variable declaration have a non-constant initializing ex-
    pression? Explain why.
4. Suppose B-Minor included aswitchstatement. Sketch out two dif-
    ferent assembly language templates for implementing aswitch.
5. Write the complete code generator for the X86-64 architecture, as out-
    lined in this chapter.
6. Write several test programs to test all the aspects of B-Minor then
    use your compiler to build, test, and run them.
7. Compare the assembly output of your compiler on your test pro-
    grams to the output of a production compiler likegccon equivalent
    programs written in C. What differences do you see?
8. Add an extra code generator to your compiler that emits a different
    assembly language like ARM or an intermediate representation like
    LLVM. Describe any changes in approach that were necessary.


##### 195

## Chapter 12 – Optimization

### 12.1 Overview

Using the basic code generation strategy shown in the previous chapter,
you can build a compiler that will produce perfectly usable, working code.
However, if you examine the output of your compiler, there are many ob-
vious inefficiencies. This stems from the fact that the basic code generation
strategy considers each program element in isolation and so must use the
most conservative strategy to connect them together.
In the early days of high level languages, before optimization strategies
were widespread, code produced by compilers was widely seen as inferior
to that which was hand-written by humans. Today, a modern compiler
has many optimization techniques and very detailed knowledge of the
underlying architecture, and so compiled code is usually (but not always)
superior to that written by humans.
Optimizations can be applied at multiple stages of the compiler. It’s
usually best to solve a problem at the highest level of abstraction possible.
For example, once we have generated concrete assembly code, about the
most we can do is eliminate a few redundant instructions. But, if we work
with a linear IR, we can speed up a long code sequence with smart reg-
ister allocation. And if we work at the level of a DAG or an AST, we can
eliminate entire chunks of unused code.
Optimizations can occur at different scopes within a program. **Local
optimizations** refer to changes that are limited to a single basic block,
which is a straight-line sequence of code without any flow control. **Global
optimizations** refer to changes applied to the entire body of a function (or
procedure, method, etc.), consisting of a control-flow-graph where each
node is a basic block. **Interprocedural optimizations** are even larger, and
take into account the relationships between different functions. Generally,
optimizations at larger scopes are more challenging but have more poten-
tial to improve the program.
This chapter will give you a tour of some common code optimization
techniques that you can either implement in your project compiler, or ex-
plore by implementing them by hand. But this is just an introduction: code
optimization is a very large topic that could occupy a whole second text-
book, and is still an area of active research today. If this chapter appeals to


##### 196 CHAPTER 12. OPTIMIZATION

you, then check out some of the more advanced books and articles refer-
enced at the end of the chapter.

### 12.2 Optimization in Perspective

As a programmer – the user of a compiler – it’s important to keep a sense
of perspective about compiler optimizations. Most production compilers
do not perform any major optimizations when run with the default op-
tions. There are several reasons for this. One reason is compilation time:
when initially developing and debugging a program, you want to be able
to edit, compile, and test many times rapidly. Almost all optimizations re-
quire additional compilation time, which doesn’t help in the initial devel-
opment phases of a program. Another reason is that not all optimizations
automatically improve a program: they may cause it to use more memory,
or even run longer! Yet another reason is that optimizations can confuse
debugging tools, making it difficult to relate program execution back to
the source code.
Thus, if you have a program that doesn’t run as fast as you would like,
it’s best to stop and think about your program from first principles. Two
suggestions to keep in mind:

- Examine the overall algorithmic complexity of your program: a bi-
    nary search (O(logn)) is always going to outperform a linear search
    (O(n)) for sufficiently largen. Improving the high level approach of
    your program is likely to yield much greater benefits than a low-level
    optimization.
- Measure the performance of your program. Use a standard profiling
    tool likegprof[4]to measure where, exactly, your program spends
    most of its time, and then focus your efforts on improving that one
    piece of code by either rewriting it, or enabling the appropriate com-
    piler optimizations.

Once your program is well-written from first principles, then it is time
to think about enabling specific compiler optimizations. Most optimiza-
tions are designed to target a certain pattern of behavior in code, and so
you may find it helpful to write your code in those easily-identified pat-
terns. In fact, most of the patterns discussed below can be performed by
hand without the compiler’s help, allowing you to do a head-to-head com-
parison of different code patterns.
Of course, the fragments of code presented in this chapter are all quite
small, and thus are only significant if they are performed a large number
of times within a program. This typically happens inside one or more
nested loops that constitute the main activity of a program, often called
the **kernel** of a computation. To measure the cost of, say, multiplying two


##### 12.3. HIGH LEVEL OPTIMIZATIONS 197

#include <time.h>

struct timeval start,stop,elapsed;
gettimeofday(&start,0);

for(i=0;i<1000000;i++) {
x = x * y;
}

gettimeofday(&stop,0);
timersub(&stop,&start,&elapsed);

printf("elapsed: %d.%06d sec",
elapsed.tv_sec,elapsed.tv_usec);

```
Figure 12.1: Timing a Fast Operation
```
values together, perform it one million times within a timer interval, as
shown in Figure 12.1.
Careful: The timer will count not only the action in the loop, but also
the code implementing the loop, so you need to normalize the result by
subtracting the runtime of an empty loop.

### 12.3 High Level Optimizations

#### 12.3.1 Constant Folding

Good programming practices often encourage the liberal use of named
constants throughout to clarify the purpose and meaning of values. For
example, instead of writing out the obscure number 86400 , one might
write out the following expression to yield the same number:

const int seconds_per_minute=60;
const int minutes_per_hour=60;
const int hours_per_day=24;

int seconds_per_day = seconds_per_minute

```
* minutes_per_hour
* hours_per_day;
```
The end result is the same ( 86400 ) but the code is much clearer about
the purpose and origin of that number. However, if translated literally, the
program would contain three excess constants, several memory lookups,
and two multiplications to obtain the same result. If done in the inner
loop of a complex program, this could be a significant waste. Ideally, it


##### 198 CHAPTER 12. OPTIMIZATION

struct expr * expr_fold( struct expr*e )
{
expr_fold( e->left )
expr_fold( e->right )

```
if( e->left and e->right are both constants ) {
```
```
f = expr_create( EXPR_CONSTANT );
f->value = e->operator applied to
e->left->value and e->right->value
expr_delete(e->left);
expr_delete(e->right);
```
return f;
} else {
return e;
}
}

```
Figure 12.2: Constant Folding Pseudo-Code
```
should be possible for the programmer to be verbose without resulting in
an inefficient program.

**Constant folding** is the technique of converting an expression (or part
of an expression) by combining multiple constants into a single constant.
An operator node in the tree with two constant child nodes can be con-
verted into a single node with the result of the operation computed in
advance. The process can cascade up so that complex expressions may
be reduced to a single constant. In effect, it moves some of the program’s
work from execution-time to compile-time.

This can be implemented by a recursive function that performs a post
order traversal of the expression tree. Figure 12.2 gives pseudo code for
constant folding on the AST.

One must be careful that the result computed in advance isprecisely
equal to what would have been performed at runtime. This requires using
variables of the same precision and dealing with boundary cases such as
underflow, overflow, and division by zero. In these cases, it is typical to
force a compile-time error, rather than compute an unexpected result.

While the effects of constant folding may seem minimal, it often is the
first step in enabling a chain of further optimizations.


##### 12.3. HIGH LEVEL OPTIMIZATIONS 199

#### 12.3.2 Strength Reduction

**Strength reduction** is the technique of converting a special case of an ex-
pensive operation into a less expensive operation. For example, the source
code expressionxˆyfor exponentiation on floating point values is, in gen-
eral, implemented as a call to the functionpow(x,y), which might be
implemented as an expansion of a Taylor series. However, in the particu-
lar case ofxˆ2we can substitute the expressionx*xwhich accomplishes
the same thing. This avoids the extra cost of a function call and many loop
iterations. In a similar way, multiplication/division by any power of two
can be replaced with a bitwise left/right shift, respectively. For example,
x* 8 can be replaced withx<<3.
Some compilers also contain rules for strength reduction of operations
in the standard library. For example, recent versions ofgccwill substi-
tute a call toprintf(s)with a constant stringsto the equivalent call
toputs(s). In this case, the strength reduction comes from reducing the
amount of code that must be linked into the program:putsis very simple,
whileprintfhas a large number of features and further code dependen-
cies.^1

#### 12.3.3 Loop Unrolling

Consider the common construct of using a loop to compute variations of
a simple expression many times:

for(i=0;i<400;i++) {
a[i] = i*2 + 10;
}

In any assembly language, this will only require a few instructions in
the loop body to compute the value ofa[i]each time. But, the instruc-
tions needed to control the loop will be a significant fraction of the execu-
tion time: each time through the loop, we must check whetheri<400and
jump back to the top of the loop.
**Loop unrolling** is the technique of transforming a loop into another
that has fewer iterations, but does more work per iteration. The number of
repetitions within the loop is known as the **unrolling factor**. The example
above could be safely transformed to this:

for(i=0;i<400;i+=4) {
a[i] = i*2 + 10;
a[i+1] = (i+1)*2 + 10;
a[i+2] = (i+2)*2 + 10;

(^1) While there is a logic to this sort of optimization, it does seem like an unseemly level of
familiarity between the compiler and the standard library, which may have different devel-
opers and evolve independently.


##### 200 CHAPTER 12. OPTIMIZATION

a[i+3] = (i+3)*2 + 10;
}

```
Or this:
```
for(i=0;i<400;i++) {
a[i] = i*2 + 10;
i++;
a[i] = i*2 + 10;
i++;
a[i] = i*2 + 10;
i++;
a[i] = i*2 + 10;
}

Increasing the work per loop iteration saves some unnecessary eval-
uations ofi<400, and it also eliminates branches from the instruction
stream, which avoids pipeline stalls and other complexities within the mi-
croprocessor.
But how much should a loop be unrolled? The unrolled loop could
contain 4, 8, 16 or even more items per iteration. In the extreme case,
the compiler could eliminate the loop entirely and replace it with a finite
sequence of statements, each with a constant value:

a[0] = 0 + 10;
a[1] = 2 + 10;
a[2] = 4 + 10;

As the unrolling factor increases, unnecessary work in the loop struc-
tures are eliminated. However, the increasing code size has its own cost:
instead of reading the same instructions over and over again, the processor
must keep loading new instructions from memory. If the unrolled loop re-
sults in a working set larger than the instruction cache, then performance
may endworsethan the original code.
For this reason, there are no hard-and-fast rules on when to use loop
unrolling. Compilers often have global options for unrolling that can be
modified by a#pragmaplaced before a specific loop. Manual experimen-
tation may be needed to get good results for a specific program.^2

#### 12.3.4 Code Hoisting

Sometimes, code fragments inside a loop are constant with each iteration
of the loop. In this case, it is unnecessary to recompute on every iteration,

(^2) The GCC manual has this to say about the-funroll-all-loopsoption: “This usually
makes programs run more slowly.”


##### 12.3. HIGH LEVEL OPTIMIZATIONS 201

and so the code can be moved to the block preceding the loop, which is
known as **code hoisting**. For example, the array index in this example is
constant throughout the loop, and can be computed once before the loop
body:

for(i=0;i<400;i++) {
a[x*y] += i;
}

```
t = x*y;
for(i=0;i<400;i++) {
a[t] += i;
}
```
Unlike loop unrolling, code hoisting is a relatively benign optimiza-
tion. The only cost of the optimization is that the computed result must
occupy either a temporary location for the duration of the loop (which
slightly increases either register pressure) or local storage consumption.
This is offset by the elimination of unnecessary computation.

#### 12.3.5 Function Inlining

**Function inlining** is the process of substituting a function call with the
effect of that function call directly in the code. This is particularly useful
for brief functions that exist to improve the clarity or modularity of code,
but do not perform a large amount of computation. For example, suppose
that the simple functionquadraticis called from many times within a
loop, like this:

int quadratic( int a, int b, int x ) {
return a*x*x + b*x + 30;
}

for(i=0;i<1000;i++) {
y = quadratic(10,20,i*2);
}

The overhead of setting up the parameters and invoking the function
likely exceeds the cost of doing the handful of additions and multiplies
within the function itself. By inlining the function code into the loop, we
can improve the overall performance of the program.
Function inlining is most easily performed on a high level represen-
tation such as an AST or a DAG. First, the body of the function must be
duplicated, then the parameters of the invocation must be substituted in.
Note that, at this level of evaluation, the parameters are not necessarily
constants, but may be complex expressions that contain unbound values.
For example, the invocation ofquadraticabove can be substituted
with the expression(a*x*x+b*x+30)under the binding ofa=10,b=20,
andx=i* 2. Once this substitution is performed, unbound variables such
asiare relative to the scope wherequadraticwas called, not where it
was defined. The resulting code looks like this:


##### 202 CHAPTER 12. OPTIMIZATION

for(i=0;i<1000;i++) {
y = 10*(i*2)*(i*2) + 20*(i*2) + 30;
}

This example highlights a hidden potential cost of function inlining: an
expression (likei* 2 ) which was previously evaluated once and then used
as a parameter to the function, is now evaluated multiple times, which
could increase the cost of the expression. On the other hand, this expan-
sion could be offset by algebraic optimizations which now have the oppor-
tunity to simplify the combination of the function with its concrete param-
eters. For example, constant folding applied to the above example yields
this:

for(i=0;i<1000;i++) {
y = 40*i*i + 40*i + 30;
}

Generally speaking, function inlining is best applied to simple leaf
functions that are called frequently and do little work relative to the cost
of invocation. However, making this determination automatically is chal-
lenging to get right, because the benefits are relatively clear, but the costs
in terms of increased code size and duplicated evaluations are not so easy
to quantify. As a result, many languages offer a keyword (likeinlinein
C and C++) that allow the programmer to make this determination manu-
ally.

#### 12.3.6 Dead Code Detection and Elimination

It is not uncommon for a compiled program to contain some amount of
code that is completely unreachable and will not be executed under any
possible input. This could be as simple as a mistake by the programmer,
who by accident returned from a function before the final statement. Or,
it could be due to the application of multiple optimizations in sequence
that eventually result in a branch that will never be executed. Either way,
the compiler can help by flagging it for the programmer or removing it
outright.

Dead code detection is typically performed on a **control flow graph**
after constant folding and other expression optimizations have been per-
formed. For example, consider the following code fragment and its control
flow graph:


##### 12.3. HIGH LEVEL OPTIMIZATIONS 203

```
if( x<y ) {
return 10;
} else {
print "hello";
}
print "goodbye";
return 30;
```
```
if
x<y
```
```
T return 10
```
```
print "hello"
```
```
F print "goodbye" return 30
```
Areturnstatement causes an immediate termination of the function
and (from the perspective of the control flow graph) is the end of the ex-
ecution path. Here, the true branch of theifstatement immediately re-
turns, while the false branch falls through to the next statement. Forsome
values ofxandy, it is possible to reach every statement.
However, if we make a slight change, like this:

```
if( x<y ) {
return 10;
} else {
return 20;
}
print "goodbye";
return 30;
```
```
if
x<y
```
```
T return 10
```
```
return 20
```
```
F print "hello" return 30
```
Then, both branches of the if statement terminate in areturn, and it
is not possible to reach the finalprintandreturnstatements. This is
(likely) a mistake by the programmer and should be flagged.
Once the control flow graph is created, determining reachability is sim-
ple: perform a traversal of the CFG, starting from the entry point of the
function, marking each node as it is visited. Once the traversal is com-
plete, any unmarked nodes are known to be unreachable. The compiler
may either generate a suitable error message or simply not generate code
for the unreachable portion.^3
Reachability analysis becomes particularly powerful when combined
with other forms of static analysis. In the example above, suppose that
variablesxandyare defined as constants 100 and 200 , respectively. Con-
stant folding can reducex<yto simplytrue, with the result that the false
branch of the if statement is never taken and therefore unreachable.
Now, don’t get carried away with this line of thinking. If you were
paying attention in your theory-of-computing course, this may sound sus-
piciously like the Halting Problem: can we determine whether anarbitrary
program written in a Turing-complete language will run to completion,
without actually executing it? The answer is, of course,no, not in the gen-
eral case. Reachability analysis simply determinesin some limited casesthat

(^3) A slight variation on this technique can be used to evaluate whether every code path
through a function results in a suitablereturnstatement. This is left as an exercise to the
reader.


##### 204 CHAPTER 12. OPTIMIZATION

a certain branch of a program is impossible to take, regardless of the pro-
gram input. It doesnotstate that the “reachable” branches of the program
willbe taken for some input, or for any input at all.

### 12.4 Low-Level Optimizations

All of the optimizations discussed so far can be applied to the high level
structure of a program, without taking into account the particular target
machine. Low-level optimizations focus more on the translation of the
program structure in a way that best exploits the peculiarities of the un-
derlying machine.

#### 12.4.1 Peephole Optimizations

**Peephole optimizations** refer to any optimization that looks very nar-
rowly at a small section of code – perhaps just two or three instructions

- and makes a safe, focused change within that section. These sort of opti-
mizations are very easy to implement as the final stage of compilation, but
have a limited overall effect.
    **Redundant load elimination** is a common peephole optimization. A
sequence of expressions that both modifies and uses the same variable can
easily result in two adjacent instructions that save a register into memory,
and then immediately load the same value again:

```
Before:
```
```
MOVQ %R8, x
MOVQ x, %R8
```
```
After:
```
```
MOVQ %R8, x
```
A slight variation is that a load to a different register can be converted
into a direct move between registers, thus saving an unnecessary load and
pipeline stall:

```
Before:
```
```
MOVQ %R8, x
MOVQ x, %R9
```
```
After:
```
```
MOVQ %R8, x
MOVQ %R8, %R9
```
#### 12.4.2 Instruction Selection

In Chapter 11, we presented a simple method of code generation where
each node of the AST (or DAG) was replaced with at least one instruc-
tion (and in some cases, multiple instructions). In a rich CISC instruction
set, a single instruction can easily combine multiple operations, such as
dereferencing a pointer, accessing memory, and performing an arithmetic
operation.
To exploit these powerful instructions, we can use the technique of in-
struction selection by **tree coverage**. [5]The idea is to first represent each


##### 12.4. LOW-LEVEL OPTIMIZATIONS 205

possible instruction in the architecture as a template tree, where the leaves
can be registers, constants, or memory addresses that can be substituted
into an instruction.
For example, one variant of the X86ADDQinstruction can add two reg-
isters together. This can be used to implement anIADDnode in the DAG,
provided the leaves of theIADDare stored in registers. Once the add is
complete, theADDQplaces the result in the same register as the second ar-
gument. This is all expressed as tree fragment that matches a part of the
DAG, and an instruction to be emitted, once specific register numbers are
chosen:

```
ADDQ Cj,Ri
```
```
RiIADD
```
```
Ri Cj
```
Figure 12.3 gives a few more examples of X86 instructions that can be
represented as tree templates. The simple instructions at the top simply
substitute one entity in the DAG for another:MOV $Cj, Riconverts a
constant into a register, whileMOV Mx, Riconverts a memory location
into a register. Richer instructions have more structure: the complex load
MOV Cj(Rl,8), Rican be used to represent a combination of add, mul-
tiply, and dereference.
Of course, Figure 12.3 is not the complete X86 instruction set. To de-
scribe even a significant subset would require hundreds of entries, with
multiple entries per instruction to capture the multiple variations on each
instruction. (For example, you would need one template for anADDQon
two registers, and another for a register-memory combination.) But this
is a feasible task and perhaps easier to accomplish than hand-writing a
complete code generator.
With the complete library of templates written, the job of the code gen-
erator is to examine the tree for sub-trees that match an instruction tem-
plate. When one is found, the corresponding instruction is emitted (with
appropriate substitutions for register numbers, etc.) and the matching por-
tion of the tree replaced with the right side of the template.
For example, suppose we wish to generate X86 code for the statement
a[i] = b + 1;Let us suppose thatbis a global variable, whileaand
iare local variables at positions 40 and 32 above the base pointer, respec-
tively. Figure 12.4 shows the steps of tree rewriting. In each DAG, the box
indicates the subtree that matches a rule in Figure 12.3.
**Step 1:** The leftIADDshould be executed first to compute the value
of the expression. Looking at our table of templates, there is noIADDthat
can directly add a memory location to a constant. So, we instead select rule
(2), which emits the instructionMOVQ b, %R0and converts the left-hand


##### 206 CHAPTER 12. OPTIMIZATION

```
RiCj^1 :MOVQ$Cj,Ri
```
```
RiMx^2 :MOVQ Mx,Ri
```
```
RiDEREF
```
```
Rj
```
```
3 :MOVQ(Rj),Ri
```
```
RiIADD
```
```
Ri Cj
```
```
4 :ADDQ Cj,Ri
```
```
RiDEREF
```
```
IADD
```
```
Rk Cj
```
```
5 :MOVQ Cj(Rk),Ri
```
```
6 :LEAQ Cj(RBP),Ri
```
```
RiIADD
```
```
RBP Cj
```
```
7 :MOVQ Ri,(Rk,Rl, 8 )
```
```
Ri
```
```
IADD
```
```
IMUL Rk
```
```
8
```
```
ASSIGN
```
```
Ri DEREF
```
```
Rl
```
```
Figure 12.3: Example X86 Instruction Templates
```
side of the template (a memory location) into the right hand side (register
%R0).
**Step 2:** Now we can see anIADDof a register and a constant, which
matches the template of rule (4). We emit the instructionADDQ $1, %R0
and replace theIADDsubtree with the register%R0.

**Step 3:** Now let’s look at the other side of the tree. We can use rule
(5) to match the entire subtree that loads the variableifrom%RBP+32by
emitting the instructionMOVQ 32(%RBP), %R1and replacing the subtree
with the register%R1.
**Step 4:** In a similar way, we can use rule (6) to compute the address ofa
by emittingLEAQ 40(%RBP), %R2. Notice that this is, in effect, a three-
address addition specialized for use with a register and the base pointer.
Unlike rule 4, it does not modify the source register.
**Step 5:** Finally, template rule (7) matches most of what is remaining.
We can emitMOVQ %R0,(%R2,%R1,8)which stores the value in R1 into
the computed array address ofa. The left side of the template is replaced
with the right-hand side, leaving nothing but the register%R0. With the
tree completely reduced to a single register, code generation is complete,
and register%R0can be freed.


##### 12.5. REGISTER ALLOCATION 207

```
IADD
```
```
b 1
```
```
IADD
```
```
32 RBP
```
```
IADD
```
```
DEREF 40 RBP
```
```
ASSIGN
```
```
DEREF
```
```
IADD
```
```
IMUL
```
```
8
```
```
IADD
```
```
R0 1
```
```
IADD
```
```
32 RBP
```
```
IADD
```
```
DEREF 40 RBP
```
```
ASSIGN
```
```
DEREF
```
```
IADD
```
```
IMUL
```
```
8
```
```
IADD
```
```
32 RBP
```
```
IADD
```
```
DEREF 40 RBP
```
```
ASSIGN
```
```
R0 DEREF
```
```
IADD
```
```
IMUL
```
```
8
```
```
IADD
```
```
40 RBP
```
```
ASSIGN
```
```
R0 DEREF
```
```
IADD
```
```
IMUL
```
```
R1 8
```
```
ASSIGN
```
```
R0 DEREF
```
```
IADD
```
```
R2 IMUL
```
```
R1 8
```
```
Figure 12.4: Example of Tree Rewriting
```
Under the simple code generation scheme, this 16-node DAG would
have produced (at least) 16 instructions of output. By using tree coverage,
we reduced the output to these five instructions:

MOVQ b,%R0
ADDQ $1,%R0
MOVQ 32(%RBP),%R1
LEAQ 40(%RBP),%R2
MOVQ %R0,(%R2,%R1,8)

### 12.5 Register Allocation

In modern CPUs, on-chip computational speed far outstrips memory la-
tency: thousands of arithmetic operations can be completed in the time


##### 208 CHAPTER 12. OPTIMIZATION

it takes to perform a single load or store. It follows that any optimiza-
tions that eliminates a load or store from memory can have a considerable
impact on the performance of a program. Eliminating completely unnec-
essary code and variables is the first step towards doing this.
The next step is to assign specific local variables into registers, so they
never need to be loaded from or stored to memory. Of course, there are
a limited number of registers and not every variable can claim one. The
process of **register allocation** is to identify the variables that are the best
candidates for locating in registers instead of memory.
The mechanics of converting a variable into a register are straightfor-
ward. In each case where a value would be loaded from or stored into a
memory location, the compiler simply substitutes the assigned register as
the location of the value, so that it is used directly as the source or target
of an instruction. The more complicated questions relate to whether it is
safeto registerize a variable, which variables are mostimportantto register-
ize, and which variables can coexist in registers at once. Let’s look at each
question in turn.

#### 12.5.1 Safety of Register Allocation

It is unsafe to registerize a variable if the eliminated memory access has
some important side effect or visibility outside of the code under consid-
eration. Examples of variables that should not be registerized include:

- Global variables shared between multiple functions or modules.
- Variables used as communication between concurrent threads.
- Variables accessed asynchronously by interrupt handlers.
- Variables used as memory-mapped I/O regions.

Note that some of these cases are more difficult to detect than others!
Globally shared variables are already known to the compiler, but the other
three cases are (often) not reflected in the language itself. In the C lan-
guage, one can mark a variable with thevolatilekeyword to indicate
that it may be changed by some method unknown to the compiler, and
therefore clever optimizations should not be undertaken. Low level code
found in operating systems or parallel programs is often compiled without
such optimizations, so as to avoid these problems.

#### 12.5.2 Priority of Register Allocation

For a small function, it may be possible to registerizeallthe variables, so
that memory is not used at all. But for even a moderately complex function
(or a CPU that has few available registers) it is likely that the compiler
must choose a limited number of variables to registerize.


##### 12.5. REGISTER ALLOCATION 209

Before automatic register allocation was developed, the programmer
was responsible for identifying such variables manually. For example, in
early C compilers, one could add theregisterkeyword to a variable
declaration, forcing it to be stored in a register. This was typically done
for the index variable of the inner-most loop. Of course, the programmer
might not choose the best variable, or they might choose too many register
variables, leaving too few available for temporaries. Today, theregister
keyword is essentially ignored by C compilers, which are capable of mak-
ing informed decisions.
What strategy should we use to automatically pick variables to reg-
isterize? Naturally, those that experience the most number of loads and
stores as the program runs. One could profile an execution of the program,
count the memory accesses per variable, and then go back and select the
topnvariables. Of course, that would be a very slow and expensive pro-
cedure for optimizing a program, but one might conceivably go about it
for a very performance-critical program.

A more reasonable approach would be to score variables via static anal-
ysis with some simple heuristics. In a linear sequence of code, each vari-
able can be directly scored by the number of loads and stores it performs:
the variable with the highest score is the best candidate. However, a vari-
able access that appears inside a loop is likely to have a much higher access
count. How large, we cannot say, but we can assume that a loop (and each
nesting of a loop) multiplies the importance of a variable by a large con-
stant. Multiply-nested loops increase importance in the same way.

#### 12.5.3 Conflicts Between Variables

Not every variable needs a distinct register. Two variables can be assigned
to the same register if their uses do not conflict. To determine this, we
must first compute the **live ranges** of each variable and then construct a
conflict graph. Within a basic block of a linear IR, a variable is live from
its first definition until its final use. (If the same code is expressed in SSA
form, then each version of a variable can be treated independently, with
its own live range.)

Now, each variable with an overlapping range cannot share the same
register, because they must exist independently. Conversely, two variables
that do not have an overlapping live range can be assigned the same reg-
ister. We can construct a **conflict graph** where each node in the graph rep-
resents a variable, and then add edges between nodes whose live ranges
overlap. Figure 12.5 gives an example of a conflict graph.

Register allocation now becomes an instance of the **graph coloring**
problem.[7]The goal is to assign each node in the graph a different color
(register) such that no two adjacent nodes have the same color. A planar
graph (like a two-dimensional political map) can always be colored with


##### 210 CHAPTER 12. OPTIMIZATION

```
Code Live Variables
```
1. x = 10; x
2. y = 10; x, y
3. a = x * y; x, y, a
4. b = a * a; x, a, b
5. a = b + 30; x, a, b
6. x = x + 1; x,
7. z = x * x; x, z
8. return z; z

```
x
```
```
y
a
b
```
```
z
```
```
Figure 12.5: Live Ranges and Register Conflict Graph
```
four colors.^4 However, a register conflict graph is not necessarily planar,
and so may require a large number of colors. The general problem of find-
ing the minimum number of colors needed is NP-complete, but there are
a number of simpler heuristics that are effective in practice.
A common approach is to sort the nodes of the graph by the number
of edges (conflicts), and then assign registers to the most conflicted node
first. Then, proceeding down the list, assign each node a register that is not
already taken by an adjacent node. If at any point, the number of available
registers is exhausted, then mark that node as a non-registerized variable,
and continue, because it may be still possible to assign registers to nodes
with fewer conflicts.

#### 12.5.4 Global Register Allocation

The procedure above describes the analysis of live variables and register
allocation for individual basic blocks. However, if each basic block is al-
located independently, it would be very difficult to combine basic blocks,
because variables would be assigned to different registers, or none at all.
It would become necessary to introduce code between each basic block to
move variables between registers or to/from memory, which could defeat
the benefits of allocation in the first place.
In order to perform global register allocation across an entire function
body, we must do so in a way that keeps assignments consistent, no matter

(^4) This mathematical problem has a particularly colorful (ahem) history. In 1852, Francis
Guthrie conjectured that only four colors were necessary to color a planar graph, while at-
tempting to color a map of Europe. He brought this problem to Augustus De Morgan, who
popularized it, leading to several other mathematicians who published several (incorrect)
proofs in the late 1800s. In 1891, Percy John Heawood proved that no more thanfivecolors
were sufficient, and that’s where things stood for the next 85 years. In 1976, Kenneth Appel
and Wolfgang Haken produced a computer-assisted proof of the four-color theorem, but the
proof contained over 400 pages of case-by-case analysis which had to be painstakingly ver-
ified by hand. This caused consternation in the mathematical community, partly due to the
practical difficulty of verifying such a result, but also because this proof was unlike any that
had come before. Does it really count as a “proof” if it cannot be easily contemplated and
verified by a human?[2]


##### 12.6. OPTIMIZATION PITFALLS 211

a=10;
for(i=0;i<10;i++) {
a = a*3;
}
x = a*2;
print x;
print a;

```
Block A:
i=0a=10
```
```
Block B:
i<10?
```
```
Block C:i++
a=a*3
```
```
T
Block D:x=a*2
print xprint a
```
```
F
```
```
i
{ABC}
```
```
a
{ABCD}
```
```
x
{D}
```
**Figure 12.6: Example of Global Register Allocation**
To perform register allocation on code (left) consisting of more than a basic block,
build the control flow graph (middle) and then determine the blocks in which
a given variable is live. Construct a conflict graph (right) such that variables
sharing a live block are in conflict, and then color the graph.

where the control flow leads. To do this, we first construct the control flow
graph for the function. For each variable definition in the graph, trace the
possible forward paths to uses of that variable, taking into account multi-
ple paths made possible by loops and branches. If there exists a path from
definition to use, then all the basic blocks in that path are members of the
set of live basic blocks for that variable. Finally, a conflict graph may be
constructed based on the sets of live blocks: each node represents a vari-
able and its set of live basic blocks; each edge represents a conflict between
two variables whose live sets intersect. (As above, the same analysis can
be performed in SSA form for a more fine-grained register assignment.)
Figure 12.6 gives an example of this analysis for a simple code fragment.

### 12.6 Optimization Pitfalls

Now that you have seen some common optimization techniques, you
should be aware of some pitfalls common to all techniques.
**Be careful of the correctness of optimizations.** A given piece of code
must produce the same result before and after optimization, for all possi-
ble inputs. We must be particularly careful with the boundary conditions
of a given piece of code, where inputs are particularly large, or small, or
run into fundamental limitations of the machine. These concerns require
careful attention, particularly when applying general mathematical or log-


##### 212 CHAPTER 12. OPTIMIZATION

ical observations to concrete code.
For example, it is tempting to apply common algebraic transformations
to arithmetic expressions. We might transforma/x + b/xinto(a+b)/x
because these expressions are equal in the abstract world of real numbers.
Unfortunately, these two expressions arenotthe same in the concrete
world of limited-precision mathematics. Suppose thata,b, andxare 32-
bit signed integers, with a range of[− 231 ,+2^31 ). If bothaandbhave the
value 2,000,000,000, thena/5is 400,000,000 anda/5+b/5is 800,000,000.
However,a+boverflows a 32-bit register and wraps around to a negative
value, so that(a+b)/5is -58993459! An optimizing compiler must be ex-
tremely cautious that any code transformations produce the same results
underall possible valuesin the program.
**Be careful not to change external side-effects.** Many aspects of real
programs depend upon the side-effects, not the results of a computation.
This is most apparent in embedded systems and hardware drivers, where
an operating system or an application communicates with external devices
through memory-mapped registers. But, it is also the case in conventional
user-mode programs that may perform I/O or other forms of communica-
tion via system calls: awrite()system call should never be eliminated
by an optimization. Unfortunately, positively identifying every external
function that has side effects is impractical. An optimizing compiler must
conservatively assume that any external functionmighthave a side effect,
and leave it untouched.
**Be careful of how optimization changes debugging.** A program com-
piled with aggressive optimizations can show surprising behavior when
run under a debugger. Statements may be executed in a completely dif-
ferent order than stated in the program, giving the impression that the
program flow jumps forwards and backwards without explanation. En-
tire parts of the program may be skipped entirely, if they are determined
to be unreachable, or have been simplified away. Variables mentioned in
the source might not exist in the executable at all. Breakpoints set on func-
tion calls may never be reached, if the function has been inlined. In short,
many of the things observable by a debugger are not really program re-
sults, but hidden internal state and are not guaranteed to appear in the
final executable program. If you expect your program to have bugs – and
it will – better fix them before enabling optimizations.

### 12.7 Optimization Interactions

Multiple optimizations can interact with each other in ways that are unpre-
dictable. Sometimes, these interactions cascade in positive ways: constant
folding can support reachability analysis, resulting in dead code elimi-
nation. On the other hand, optimizations can interact in negative ways:
function inlining can result in more complex expressions, resulting in less
efficient register allocation. What’s worse is that one set of optimizations


##### 12.7. OPTIMIZATION INTERACTIONS 213

may be very effective on one program, but counter-productive on another.
A modern optimizing compiler can easily have fifty different optimiza-
tion techniques of varying complexity. If it is only a question of turning
each optimization on or off, then there are (only) 250 combinations. But, if
the optimizations can be applied in any order, there arefact(50)permuta-
tions! How is the user to decide which optimizations to enable?
Most production compilers define a few discrete levels of optimization.
For example,gccdefines-O0as the fastest compile speed, with no opti-
mization,-O1enables about thirty optimizations with modest compile-
time expense and few runtime drawbacks (e.g. dead code elimination),
-O2enables another thirty optimizations with greater compile-time ex-
pense (e.g. code hoisting), and-O3enables aggressive optimizations that
may or may not pay off (e.g. loop unrolling). On top of that, individual
optimizations may be turned on or off manually.
But is it possible to do better with finer-grained control? A number of
researchers have explored methods of finding the best combinations of op-
timizations, given a benchmark program that runs reasonably quickly. For
example, the CHiLL[8]framework combines parallel execution of bench-
marks with a high level heuristic search algorithm to prune the overall
search space. Another approach is to use genetic algorithms[9]in which
a representative set of configurations is iteratively evaluated, mutated, re-
combined, until a strong configuration emerges.


##### 214 CHAPTER 12. OPTIMIZATION

### 12.8 Exercises

1. To get a good sense of the performance of your machine, follow the
    advice of Jon Bentley[1]and write some simple benchmarks to mea-
    sure these fundamental operations:

```
(a) Integer arithmetic.
(b) Floating point arithmetic.
(c) Array element access.
(d) A simple function call.
(e) A memory allocation.
(f) A system call likeopen().
```
2. Obtain a standard set of benchmark codes (such as SPEC) and evalu-
    ate the effect of various optimization flags available on your favorite
    compiler.
3. Implement the constant folding optimization in the AST of your project
    compiler.
4. Identify three opportunities for strength reduction in the B-Minor
    language, and implement them in your project compiler.
5. Implement reachability analysis in your project compiler, using ei-
    ther the AST or a CFG. Use this to ensure that all flow control paths
    through a function end in a suitablereturnstatement.
6. Write code to compute and display the live ranges of all variables
    used in your project compiler.
7. Implement linear-scan register allocation[6]on basic blocks, based
    on the live ranges computed in the previous exercise.
8. Implement graph-coloring register allocation[7]on basic blocks, based
    on the live ranges computed in the previous exercise.


##### 12.9. FURTHER READING 215

### 12.9 Further Reading

1. J. Bentley, “Programming Pearls”, Addison-Wesley, 1999.
    A timeless book that offers the programmer a variety of strategies for evaluating the
    performance of a program and improving its algorithms and data structures.
2. R. Wilson, “Four Colors Suffice: How the Map Problem Was Solved”,
    Princeton University Press, 2013.
    A history of the long winding road from the four-color conjecture in the nineteenth
    century to its proof by computer in the twentieth century.
3. A. Aho, M. S. Lam, R. Sethi, J. D. Ullman, “Compilers: Principles,
    Techniques, and Tools”, 2nd edition, Pearson, 2013.
    Affectionately known as the “Dragon Book”, this is an advanced and comprehensive
    book on optimizing compilers, and you are now ready to tackle it.
4. S. L. Graham, P. B. Kessler, and M. K. McKusick. “Gprof: A call
    graph execution profiler.” ACM SIGPLAN Notices, volume 17, num-
    ber 6, 1982.https://doi.org/10.1145/872726.806987
5. A. Aho, M. Ganapathi, and S. Tjiang, “Code Generation Using Tree
    Matching and Dynamic Programming”, ACM Transactions on Pro-
    gramming Languages and Systems, volume 11, number 4, 1989.https:
    //doi.org/10.1145/69558.75700
6. M. Poletto and V. Sarkar, “Linear Scan Register Allocation”, ACM
    Transactions on Programming Languages and Systems, volume 21,
    issue 5, 1999.https://doi.org/10.1145/330249.330250
7. G. J. Chaitin, “Register Allocation & Spilling via Graph Coloring”,
    ACM SIGPLAN Notices, voume 17, issue 6, June 1982.
    https://doi.org/10.1145/800230.806984
8. A. Tiwari, C. Chen, J. Chame, M. Hall, J. Hollingsworth, “A Scalable
    Auto-tuning framework for compiler optimization”, IEEE Interna-
    tional Symposium on Parallel and Distributed Processing, 2009.
    https://doi.org/10.1109/IPDPS.2009.5161054
9. M. Stephenson, S. Amarasinghe, M. Martin, U. O’Reilly, “Meta op-
    timization: improving compiler heuristics with machine learning”,
    ACM SIGPLAN Conference on Programming Language Design and
    Implementation, 2003.
    https://doi.org/10.1145/781131.781141


##### 216 CHAPTER 12. OPTIMIZATION


##### 217

## Appendix A – Sample Course Project

This appendix describes a semester-long course project which is the sug-
gested companion to this book. Your instructor may decide to use it as-is,
or make some modifications appropriate to the time and place of your
class. The overall goal of the project is to build a complete compiler that
accepts a high level language as input and produces working assembly
code as output. It can naturally be broken down into several stages, each
one due at an interval of a few weeks, allowing for 4-6 assignments over
the course of a semester.
The recommended project is to use the B-Minor language as the source,
and X86 or ARM assembly as the output, since both are described in this
book. But you could accomplish similar goals with a different source lan-
guage (like C, Pascal, or Rust) or a different assembly language or inter-
mediate representation (like MIPS, JVM, or LLVM).
Naturally, the stages are cumulative: the parser cannot work correctly
unless the scanner works correctly, so it is important for you to get each
one right, before moving onto the next one. A critical development tech-
nique is to create a large number (30 or more) test cases for each stage, and
provide a script or some other automated means for running them auto-
matically. This will give you confidence that the compiler works across
all the different aspects of B-Minor, and that a fix to one problem doesn’t
break something else.

### A.1 Scanner Assignment

Construct a scanner for B-Minor which reads in a source file and produces
a listing of each token one by one, annotated with the token kind (iden-
tifier, integer, string, etc) and the location in the source. If invalid input
is discovered, produce a message, recover from the error, and continue.
Create a set of complete tests to exercise all of the tricky corner cases of
comments, strings, escape characters, and so forth.

### A.2 Parser Assignment

Building on the scanner, construct a parser for B-Minor using Bison (or
another appropriate tool) which reads in a source file, determines whether


##### 218 APPENDIX A. SAMPLE COURSE PROJECT

the grammar is valid, and indicates success or failure. Use the diagnostic
features of Bison to evaluate the given grammar for ambiguities and work
to resolve problems such as the dangling-else. Create a set of complete
tests to exercise all of the tricky corner cases.

### A.3 Pretty-Printer Assignment

Next, use the parser to construct the complete AST for the source program.
To verify the correctness of the AST, print it back out as an equivalent
source program, but with all of the whitespace arranged nicely so that it is
pleasant to read. This will result in some interesting discussions with the
instructor about what constitutes an “equivalent” program. A necessary
(but not sufficient) requirement is that the output program should be re-
parseable by the same tool. This requires attention to some details with
comments, strings, and general formatting. Again, create a set of test cases.

### A.4 Typechecker Assignment

Next, add methods which walk the AST and perform semantic analysis
to determine the correctness of the program. Symbol references must be
resolved to definitions, the type of expressions must be inferred, and the
compatibility of values in context must be checked. You are probably used
to encountering incomprehensible error messages from compilers: this is
your opportunity to improve upon the situation. Again, create a set of test
cases.

**A.5 Optional: Intermediate Representation**

Optionally, the project can be extended by adding a pass to convert the
AST into an intermediate representation. This could be a custom three or
four-tuple code, an internal DAG, or a well established IR such as JVM
or LLVM. The advantage of using the later is that output can be easily
fed into existing tools and actually executed, which should give you some
satisfaction. Again, create a set of test cases.

### A.6 Code Generator Assignment

The most exciting step is to finally emit working assembly code. Straight-
forward code generation is most easily performed on the AST itself, or a
DAG derived from the AST in the optional IR assignment, following the
procedure in Chapter 11. For the first attempt, it’s best not to be concerned
about the efficiency of the code, but allow each code block to conserva-
tively stand on its own. It is best to start with some extremely simple
programs (e.g. return 2+2;) and gradually add complexity bit by bit.
Here, your practice in constructing test cases will really pay off, because


##### A.7. OPTIONAL: EXTEND THE LANGUAGE 219

you will be able to quickly verify how many test programs are affected by
one change to the compiler.

### A.7 Optional: Extend the Language

In the final step, you are encouraged to develop your own ideas for ex-
tending B-Minor itself with new data types or control structures, to create
a new backend targeting a different CPU architecture, or to implement one
or more optimizations described in Chapter 12.


##### 220 APPENDIX A. SAMPLE COURSE PROJECT


##### 221

## Appendix B – The B-Minor Language

### B.1 Overview

The B-Minor language is a “little” language suitable for use in an un-
dergraduate compilers class. B-Minor includes expressions, basic control
flow, recursive functions, and strict type checking. It is object-code com-
patible with ordinary C and thus can take advantage of the standard C
library, within its defined types.
B-Minor is similar enough to C that it should feel familiar, but has
enough variations to allow for some discussion of how different language
choices affect the implementation. For example, the type syntax of B-
Minor is closer to that of Pascal or SQL than of C. Students may find this
awkward at first, but its value becomes clearer when constructing a parser
and when discussing types independently of the symbols that they apply
to. Theprintstatement gives an opportunity to perform simple type
inference and interact with runtime support. A few unusual operators
cannot be implemented in a single assembly instruction, illustrating how
complex language intrinsics are implemented. The strict type system gives
the students some experience with reasoning about rigorous type algebras
and producing detailed error messages.
A proper language definition would be quite formal, including regular
expressions for each token type, a context-free-grammar, a type algebra,
and so forth. However, if we provided all that detail, it would rob you
(the student) of the valuable experience of wrestling with those details.
Instead, we will describe the language through examples, leaving it to you
to read carefully, and then extract the formal specifications needed for your
code. You are certain to find some details and corner cases that are unclear
or incompletely specified. Use that as an opportunity to ask questions
during class or office hours and work towards a more precise specification.


##### 222 APPENDIX B. THE B-MINOR LANGUAGE

### B.2 Tokens

In B-Minor, whitespace is any combination of the following characters:
tabs, spaces, linefeed, and carriage return. The placement of whitespace is
not significant in B-Minor. Both C-style and C++-style comments are valid
in B-Minor:

/* A C-style comment */
a=5; // A C++ style comment

Identifiers (i.e. variable and function names) may contain letters, num-
bers, and underscores. Identifiers must begin with a letter or an under-
score. These are examples of valid identifiers:

i x mystr fog123 BigLongName55

The following strings are B-Minor keywords and may not be used as
identifiers:

array boolean char else false for function if
integer print return string true void while

### B.3 Types

B-Minor has four atomic types: integers, booleans, characters, and strings.
A variable is declared as a name followed by a colon, then a type and an
optional initializer. For example:

x: integer;
y: integer = 123;
b: boolean = false;
c: char = ’q’;
s: string = "hello world\n";

Anintegeris always a signed 64 bit value.booleancan take the lit-
eral valuestrueorfalse.charis a single 8-bit ASCII character.string
is a double-quoted constant string that is null-terminated and cannot be
modified. (Note that, unlike C,stringis not an array ofchar, it is a
completely separate type.)
Bothcharandstringmay contain the following backslash codes.n
indicates a linefeed (ASCII value 10), 0 indicates a null (ASCII value zero),
and a backslash followed by anything else indicates exactly the following
character. Both strings and identifiers may be up to 256 characters long.
B-Minor also allows arrays of a fixed size. They may be declared with
no value, which causes them to contain all zeros:

a: array [5] integer;

```
Or, the entire array may be given specific values:
```
a: array [5] integer = {1,2,3,4,5};


##### B.4. EXPRESSIONS 223

### B.4 Expressions

B-Minor has many of the arithmetic operators found in C, with the same
meaning and level of precedence:

```
[] f() array subscript, function call
++ -- postfix increment, decrement
```
-! unary negation, logical not
ˆ exponentiation
* / % multiplication, division, modulus
+ - addition, subtraction
< <= > >= == != comparison
&& || logical and, logical or
= assignment

B-Minor isstrictly typed. This means that you may only assign a value
to a variable (or function parameter) if the types matchexactly. You can-
not perform many of the fast-and-loose conversions found in C. For ex-
ample, arithmetic operators can only apply to integers. Comparisons can
be performed on arguments of any type, but only if they match. Logical
operations can only be performed on booleans.
Following are examples of some (but not all) type errors:

x: integer = 65;
y: char = ’A’;
if(x>y) ... // error: x and y are of different types!

f: integer = 0;
if(f) ... // error: f is not a boolean!

writechar: function void ( char c );
a: integer = 65;
writechar(a); // error: a is not a char!

b: array [2] boolean = {true,false};
x: integer = 0;
x = b[0]; // error: x is not a boolean!

```
Following are some (but not all) examples of correct type assignments:
```
b: boolean;
x: integer = 3;
y: integer = 5;
b = x<y; // ok: the expression x<y is boolean

f: integer = 0;


##### 224 APPENDIX B. THE B-MINOR LANGUAGE

if(f==0) ... // ok: f==0 is a boolean expression

c: char = ’a’;
if(c==’a’) ... // ok: c and ’a’ are both chars

### B.5 Declarations and Statements

In B-Minor, you may declare global variables with optional constant ini-
tializers, function prototypes, and function definitions. Within functions,
you may declare local variables (including arrays) with optional initial-
ization expressions. Scoping rules are identical to C. Function definitions
may not be nested.
Within functions, basic statements may be arithmetic expressions,
returnstatements,printstatements,ifandif-elsestatements,for
loops, or code within inner{}groups. B-Minor does not have switch state-
ments, while-loops, or do-while loops, since those are easily represented
as special cases offorandif.
Theprintstatement is a little unusual because it is a statement and
not a function call.printtakes a list of expressions separated by commas,
and prints each out to the console, like this:

print "The temperature is: ", temp, " degrees\n";

Note that each element in the list following aprintstatement is an
expression ofanytype. Theprintmechanism will automatically infer
the type and print out the proper representation.

### B.6 Functions

Functions are declared in the same way as variables, except giving a type
offunctionfollowed by the return type, arguments, and code:

square: function integer ( x: integer ) = {
return xˆ2;
}

The return type of a function must be one of the four atomic types,
orvoidto indicate no type. Function arguments may be of any type.
integer,boolean, andchararguments are passed by value, while
stringandarrayarguments are passed by reference. As in C, arrays
passed by reference have an indeterminate size, and so the length is typi-
cally passed as an extra argument:


##### B.7. OPTIONAL ELEMENTS 225

printarray: function void
( a: array [] integer, size: integer ) = {
i: integer;
for( i=0;i<size;i++) {
print a[i], "\n";
}
}

A function prototype states the existence and type of the function, but
includes no code. This must be done if the user wishes to call an external
function linked by another library. For example, to invoke the C function
puts:

puts: function void ( s: string );

main: function integer () = {
puts("hello world");
}

A complete program must have amainfunction that returns an inte-
ger. The arguments tomainmay either be empty, or useargcandargv
in the same manner as C. (The declaration ofargcandargvis left as an
exercise to the reader.)

### B.7 Optional Elements

Creating a complete implementation of the language above from begin-
ning to end should be more than enough to keep an undergraduate class
busy for a semester. However, if you need some additional challenge, con-
sider the following ideas:

- Add a new native typecomplexwhich implements complex num-
    bers. To make this useful, you will need to add some additional func-
    tions or operators to construct complex values, perform arithmetic,
    and extract the real and imaginary parts.
- Add a new automatic typevarwhich allows one to declare a vari-
    able without a concrete type. The compiler should infer the type
    automatically based on assignments made to that variable. Consider
    carefully what should happen if a function definition has a parame-
    ter of typevar.
- Improve the safety of arrays by making the array accesses automat-
    ically checked at runtime against the known size of the array. This
    requires making the length of the array a runtime property stored in
    memory alongside the array data, checking each array access against


##### 226 APPENDIX B. THE B-MINOR LANGUAGE

```
the boundaries, and taking appropriate action on a violation. Com-
pare the performance of checked arrays against unchecked arrays.
(The X86 BOUND instruction might be helpful.)
```
- Add a new mutable string typemutstringwhich has a fixed size,
    but can be modified in place, and can be converted to and from a
    regularstringas needed.
- Add an alternative control flow structure likeswitch, which eval-
    uates a single control expression, and then branches to alternatives
    with matching values. For an extra challenge, allowswitchto select
    value ranges, not just constant values.
- Implement structure types that allow multiple data items to be
    grouped together in a simple type. At the assembly level, this is
    not very different from implementing arrays, because each element
    is simply at a known offset from the base object. However, parsing
    and typechecking become more complicated because the elements
    associated with a structure type must be tracked.


##### 227

## Appendix C – Coding Conventions

C has been the language of choice for implementing low level systems like
compilers, operating systems, and drivers since the 1980s. However, it is
fair to say that C does not enforce a wide variety of good programming
practices, in comparison to other languages. To write solid C, you need to
exercise a high degree of self-discipline.^1
For many students, a compilers course in college is the first place where
you are asked to create a good sized piece of software, refining it through
several development cycles until the final project is reached. This is a good
opportunity for you to pick up some good habits that will make you more
productive.
To that end, here are the coding conventions that I ask my students to
observe when writing C code. Each of these recommendations requires a
little more work up front, but will save you headaches in the long run.
**Use a version control system.** There are a variety of nice open source
systems for keeping track of your source code. Today, Git, Mercurial, and
Subversion are quite popular, and I’m sure next year will bring a new one.
Pick one, learn the basic features, and shape your code gradually by mak-
ing small commits.^2
**Go from working to working.** Never leave your code in a broken state.
Begin by checking in the simplest possible sketch of your program that
compiles and works, even if it only prints out “hello world”. Then, add
to the program in a minor way, make sure that it compiles and runs, and
check it in again.^3
**Eliminate dead code.** Students often pick up the habit of commenting
out one bit of code while they attempt to change it and test it. While this

(^1) Why not use C++ to address some of these disciplines? Although C++ is a common
part of many computer science curricula, I generally discourage the use of C++ by students.
Although it has many features that are attractive at first glance, they are not powerful enough
to allow you to dispense with the basic C mechanisms. (For example, even if you use the C++
string class, you still need to understand basic character arrays and pointers.) Further, the
language is so complex that very few people really understand the complete set of features
and how they interact. If you stick with C, what you see is what you get.
(^2) Some people like to spend endless hours arguing about the proper way to use arcane
features of these tools. Don’t be one of those people: learn the basic operations and spend
your mental energy on your code instead.
(^3) This advice is often attributed as one of Jim Gray’s “Laws of Data Engineering” in slide
presentations, but I haven’t been able to find an authoratative reference.


##### 228 APPENDIX C. CODING CONVENTIONS

is a reasonable tactic to use for a quick test, don’t allow this dead code to
pile up in your program, otherwise your source code will quickly become
incomprehensible. Remove unused code, data, comments, files, and any-
thing else that is unnecessary to the program, so that you can clearly see
what it does now. Trust your version control system to allow you to go
back to a previously working version, if needed.
**Use tools to handle indenting.** Don’t waste your time arguing about
indenting style; find a tool that does it for you automatically, and then
forget about it. Your editor probably has a mode to indent automatically.
If not, use the standard Unix toolindent.
**Name things consistently.** In this book, you will see that every func-
tion consists of a noun and a verb:exprtypecheck,declcodegen,
etc. Each one is used consistently:expris always used for expressions,
codegenis always used for code generation. Every function dealing with
expressions is in theexprmodule. It may be tempting to take shortcuts
or make abbreviations in the heat of battle, but this will come back to bite
you. Do it right the first time.
**Put only the interface in a header file.** In C, a header file (likeexpr.h)
is used to describe the elements needed to call a function: function proto-
types and the types and constants necessary to invoke those functions. If a
function is only used within one module, it shouldnotbe mentioned in the
header file, because nobody outside the module needs that information.
**Put only the implementation in a source file.** In C, a source file (like
expr.c) is used to provide the definitions of functions. In the source
file, you should include the corresponding header (expr.h) so that the
compiler can check that your function definitions match the prototypes.
Any function or variable that is private to the module should be declared
static.
**Be lazy and recursive.** Many language data structures are hierarchi-
cally nested. When designing an algorithm, take note of the nested data
structures, and pass responsibility to other functions, even if you haven’t
written them yet. This technique generally results in code that is simple,
compact, and readable. For example, to print out a variable declaration,
break it down into printing the name, then the type, then the value, with
some punctuation in between:

printf("%s:\n",d->name);
type_print(d->type);
printf(" = ");
expr_print(d->value);
printf(" ;\n");

Then proceed to writingtypeprintandexprprint, if you haven’t
done them already.
**Use a Makefile to build everything automatically.** Learn how to write
a Makefile, if you haven’t already. The basic syntax of Make is very simple.


##### 229

The following rule says thatexpr.odepends uponexpr.candexpr.h,
and can be built by running the commandgcc:

expr.o: expr.c expr.h
gcc expr.c -c -o expr.o -Wall

There are many variations of Make that include wildcards, pattern sub-
stitution, and all manner of other things that can be confusing to the non-
expert. Just start by writing plain old rules whose meaning is clear.
**Null pointers are your friends.** When designing a data structure, use
null pointers to indicate when nothing is present. You cannot dereference
a null pointer, of course, and so you must check before using it. This can
lead to code cluttered with null checks everywhere, like this:

void expr_codegen( struct expr *e, FILE *output )
{
if(e->left) expr_codegen(e->left,output);
if(e->right) expr_codegen(e->right,output);

...
}

You can eliminate many of them by simply placing the check at the
beginning of the function, and programming in a recursive style:

void expr_codegen( struct expr *e, FILE *output )
{
if(!e) return;

```
expr_codegen(e->left,output);
expr_codegen(e->right,output);
```
...
}

**Automate regression testing.** A compiler has to handle a large number
of details, and it is all too easy for you to accidentally introduce a new bug
when attempting to fix an old one. To handle this, create a simple test suite
that consists of a set of sample programs, some correct and some incorrect.
Write a little script that invokes your compiler on each sample program,
and makes sure that it succeeds on the good tests, and fails on the bad
tests. Make it a part of your Makefile, so that every time you touch the
code, the tests are run, and you will know if things are still working.


##### 230 INDEX

## Index

a.out, 146
absolute address, 143
abstract syntax tree, 75
abstract syntax tree (AST), 7, 8,
85
accepting states, 16
accepts, 16
Acorn Archimedes, 167
Acorn RISC Machine, 167
address computation, 143
Advanced RISC Machine (ARM),
167
alternation, 14
ambiguous grammar, 38
ARM (Advanced RISC Machine),
167
assembler, 6
associativity, 15
AST (abstract syntax tree), 7, 8,
85
atomic types, 103

backtracking, 13
base pointer, 140
base-relative, 155
base-relative address, 143
basic block, 125
binary blob, 146
bottom-up derivation, 37
break, 136
BSS size, 147
bytecode, 1

callee saved, 160
caller saved, 160
calling convention, 141

```
canonical collection, 51
CFG (context-free grammar), 36
Chomsky hierarchy, 63
CISC (Complex Instruction Set Com-
puter), 167
closure, 51
code generator, 7
code hoisting, 201
code segment, 136
comments, 11
commutativity, 15
compact finite state machine, 51
compiler, 1, 5
complex, 155
Complex Instruction Set Computer
(CISC), 167
compound types, 104
concatenation, 14
conditional execution, 173
conflict graph, 209
constant folding, 124, 198
context free languages, 63
context sensitive languages, 64
context-free grammar (CFG), 36
control flow graph, 125, 202
core, 62
crystal ball interpretation, 18
```
```
DAG (directed acyclic graph), 120
data segment, 136
data size, 147
declaration, 85
delete, 138
derivation, 37
deterministic finite automaton (DFA),
16
```

##### INDEX 231

directed acyclic graph (DAG), 120
directives, 151
distribution, 15
domain specific languages, 2
dot, 51
dynamically typed language, 101

entry point, 147
enumerations, 104
epsilon closure, 22
evaluated, 85
executable formats, 146
explicitly typed language, 102
expression, 85
Extensible Linking Format (ELF),
147
external format, 119

FA (finite automaton), 15
finite automata, 13
finite automaton (FA), 15
frame pointer, 140
free, 138
function inlining, 201

GIMPLE (GNU Simple Represen-
tation), 130
Global data, 143
global value, 154
GNU Simple Representation (GIM-
PLE), 130
grammar, 7, 8
graph coloring, 209
guard page, 137

Heap data, 144
heap segment, 136

idempotency, 15
identifiers, 11
immediate value, 154
implicitly typed language, 102
indirect value, 155
instruction selection, 7
instructions, 151

```
intermediate representation (IR),
7, 119
interpreter, 1, 69
IR (intermediate representation),
119
items, 51
```
```
Java Virtual Machine (JVM), 132
JIT, 1
just in time compiling, 1
JVM (Java Virtual Machine), 132
```
```
kernel, 51, 196
keywords, 11
Kleene closure, 14
```
```
labels, 151
LALR (Lookahead LR), 62
language, 37
leaf function, 143, 162, 175
left recursion, 41
lifetime, 128
linker, 6
literal pool, 170
little languages, 2
live ranges, 209
LL(1) parse table, 47
Local data, 144
logical segments, 135
lookahead, 59
Lookahead LR (LALR), 62
loop unrolling, 199
LR(0) automaton, 51
```
```
magic number, 147
malloc, 138
many-worlds interpretation, 18
memory fragmentation, 139
```
```
name resolution, 99, 111
new, 138
NFA (nondeterministic finite au-
tomaton), 17
non-terminal, 36
nondeterministic finite automaton
(NFA), 17
```

##### 232 INDEX

numbers, 11

object code, 6
optimization, global, 195
optimization, interprocedural, 195
optimization, local, 195
optimization, peephole, 204
optimizers, 7

page fault, 137
parser, 7
parser generator, 69
partial execution, 124
PC-relative address, 144
preprocessor, 5

record type, 104
recursive descent parser, 45
recursively enumerable languages,
64
reduce, 50
reduce-reduce conflict, 54
Reduced Instruction Set Computer
(RISC), 167
redundant load elimination, 204
register allocation, 7, 208
register value, 154
regular expression, 14
regular expressions, 13
regular languages, 63
rejects, 16
RISC (Reduced Instruction Set Com-
puter), 167
rules, 36
runtime library, 191

safe programming language, 101
scanner, 6
scanner generator, 27
scope, 108
scratch registers, 181
section table, 147
segmentation fault, 137
semantic actions, 74
semantic routines, 7, 8
semantic type, 79

```
semantic values, 74
semantics, 99
sentence, 36
sentential form, 36
sentinel, 11
shift, 50
shift-reduce, 50
shift-reduce conflict, 54
side effect, 85
side effects, 187
Simple LR (SLR), 55
SLR (Simple LR), 55
SLR grammar, 55
SLR parse tables, 55
source language, 1
SSA (static single assignment), 127
stack, 140
stack backtrace, 177
stack frame, 140, 163
stack machine, 129
stack pointer, 140, 159, 173
stack segment, 136
start symbol, 36
statement, 85
static single assignment (SSA), 127
statically typed language, 101
strength reduction, 199
strings, 11
structure tag, 145
structure type, 104
subset construction, 22
symbol table, 99, 107, 147
System V ABI, 160
```
```
target language, 1
terminal, 36
text segment, 136
text size, 147
tokens, 6, 7, 11
toolchain, 5
top-down derivation, 37
translator, 69
tree coverage, 204
type, 100
type checking, 99
```

##### INDEX 233

typechecking, 8

union types, 104
unrolling factor, 199
unsafe programming language, 100
user-defined types, 103

validator, 69, 73
value, 85, 187
value-number method, 123
variant type, 105
virtual machine, 1
virtual registers, 128
virtual stack machine, 129

weak equivalence, 37
whitespace, 11

YYSTYPE, 79


