## Chapter 2 – A Quick Tour

### 2.1 The Compiler Toolchain

A compiler is one component in a **toolchain** of programs used to create
executables from source code. Typically, when you invoke a single com-
mand to compile a program, a whole sequence of programs are invoked in
the background. Figure 2.1 shows the programs typically used in a Unix
system for compiling C source code to assembly code.

编译器是将源代码转换为可执行文件的**工具链**中的一个组件。
通常，当您调用编译命令去编译一个程序，整个工具链都会在后端进行工作。 图 2.1 显示了 Unix 中通常使用的程序
用于将 C 源代码编译为汇编代码的系统。

![Figure 2.1: A Typical Compiler Toolchain](static/Chaptor_2/Figure%202.1_A%20Typical%20Compiler%20Toolchain.png)
Figure 2.1: A Typical Compiler Toolchain


- The **preprocessor** prepares the source code for the compiler proper.
    In the C and C++ languages, this means consuming all directives that
    start with the#symbol. For example, an #include directive causes
    the preprocessor to open the named file and insert its contents into
    the source code. A#definedirective causes the preprocessor to
    substitute a value wherever a macro name is encountered. (Not all
    languages rely on a preprocessor.)

  顾名思义，**预处理器**（`preprocessor`）会为编译器对源代码进行一系列预处理操作。
     在 C 和 C++ 语言中，这意味着会对所有以`#`开头的语句进行预处理。 例如，`#include`会使预处理器打开目标文件，并将其内容插入到源码中。
     预处理器打开命名文件并将其内容插入
     源代码。 `#define`会让预处理器对值进行替换。 值得一提的是，并非所有语言都依赖于预处理器。
- The **compiler** proper consumes the clean output of the preproces-
    sor. It scans and parses the source code, performs typechecking and
    other semantic routines, optimizes the code, and then produces as-
    sembly language as the output. This part of the toolchain is the main
    focus of this book.

    **编译器**（`compiler`）会扫描和解析之行过预处理的源代码，执行类型检查和
     其他语义例程，优化代码，然后生成汇编代码作为输出。 这一部分是本书的重点。

- The **assembler** consumes the assembly code and produces **object**
    **code**. Object code is “almost executable” in that it contains raw ma-
    chine language instructions in the form needed by the CPU. How-
    ever, object code does not know the final memory addresses in which
    it will be loaded, and so it contains gaps that must be filled in by the
    linker.

    **汇编器**（`assembler`）的输入是汇编语句，输出目标代码（`object code`）。 目标代码基本等同于可执行文件，因为它包含了CPU指令。然而，目标代码不知道它被加载后的内存地址，这时就需要链接器的帮助。

- The **linker** consumes one or more object files and library files and
    combines them into a complete, executable program. It selects the
    final memory locations where each piece of code and data will be
    loaded, and then “links” them together by writing in the missing ad-
    dress information. For example, an object file that calls theprintf
    function does not initially know the address of the function. An
    empty (zero) address will be left where the address must be used.
    Once the linker selects the memory location ofprintf, it must go
    back and write in the address at every place whereprintfis called.

    **链接器**（`linker`）使用一个或多个目标文件和库文件，作为输入，将它们组合成一个完整的可执行程序。 它会确定代码和数据被加载后的内存布局，向每一个目标文件及库文件写入地址信息，将它们链接在一起。
    例如，一个内部调用了`printf`函数的目标文件，在最初并不知道函数的地址。 在需要用到函数地址的位置，会点入一个空的地址值。一旦链接器决定了`printf`的内存位置，它会将这个地址填入到每一个调用了`printf`函数到地方。

In Unix-like operating systems, the preprocessor, compiler, assembler,
and linker are historically namedcpp,cc1,as, andldrespectively. The
user-visible program cc simply invokes each element of the toolchain in
order to produce the final executable.

在类 Unix 操作系统中，预处理器、编译器、汇编器、
和链接器在历史上分别被命名为：`cpp`、`cc1`、`as` 和 `ld`。耳熟能详的`cc`只是按顺序调用了工具链的每个组件，以生成最终的可执行文件。

### 2.2 Stages Within a Compiler

In this book, our focus will be primarily on the compiler proper, which is
the most interesting component in the toolchain. The compiler itself can
be divided into several stages:

本书主要着眼于整个工具链中最有意思（个人认为）的编译器部分。一个编译器可以被分为以下几个阶段：

![Figure 2.2: The Stages of a Unix Compiler](static/Chaptor_2/Figure%202.2_The%20Stages%20of%20a%20Unix%20Compiler.png)
Figure 2.2: The Stages of a Unix Compiler

- The **scanner** consumes the plain text of a program, and groups to-
    gether individual characters to form complete **tokens**. This is much
    like grouping characters into words in a natural language.

    **扫描器**（Scanner）以程序源代码作为输入，将字符组合为完整的**句柄**（Token）。
    就像我们将字符组合成单词一样。


- The **parser** consumes tokens and groups them together into com-
    plete statements and expressions, much like words are grouped into
    sentences in a natural language. The parser is guided by a **grammar**
    which states the formal rules of composition in a given language.
    The output of the parser is an **abstract syntax tree (AST)** that cap-
    tures the grammatical structures of the program. The AST also re-
    members where in the source file each construct appeared, so it is
    able to generate targeted error messages, if needed.

    **解析器**（parser）以句柄（token）作为输入，将它们组合成完整的语句和表达式，就像我们将单词组合为句子一样。 每一种语言都有其特定的**语法**（grammer）来告诉解析器如何将字符组合为句子。
     解析器的输出是一个**抽象语法树 (Abstract Syntax Tree，AST)**，它可以表示程序语法结构的。 
     AST记录着每一个语意结构在源文件中出现的位置，因此，如有需要，解析器能够生成有针对性的错误消息。

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

    **语义例程**会遍历 AST 并从程序规则及程序元素之间的联系中，获得额外的语义信息。
    比如，，我们可以根据对变量x的声明：`float x = 3;`，来判断`x + 10`这一表达式应当适用int与float相加的规则，其结果应当为一个浮点类型。
    在执行完语义例程后，AST经常会被转换为**中间表示（Intermediate Representation，IR）**，
    它可以被理解为简化版的汇编语义，方便编译器对程序进行近进一步的分析优化。
    我们将在第 8 章进一步讨论 IR。


- One or more **optimizers** can be applied to the intermediate represen-
    tation, in order to make the program smaller, faster, or more efficient.
    Typically, each optimizer reads the program in IR format, and then
    emits the same IR format, so that each optimizer can be applied in-
    dependently, in arbitrary order.

    一个或多个**优化器（Optimizer）**可以被应用于IR，以使程序更小、更快或更高效。
    通常，每个优化器的输入及输出都使用IR格式，因此每个优化器可以以任意顺序，独立地对程序进行优化。

- Finally, a **code generator** consumes the optimized IR and transforms
    it into a concrete assembly language program. Typically, a code gen-
    erator must perform **register allocation** to effectively manage the
    limited number of hardware registers, and **instruction selection** and
    **sequencing** to order assembly instructions in the most efficient form.

    最后，**代码生成器（Code Generator）**使用优化后的 IR ，并将其转化为具体的汇编语言程序。
    通常，代码生成器必须进行**寄存器分配**，以高效地管理有限数量的硬件寄存器。并且进行**指令筛选**和**排序**，来以最有效率的方式执行汇编指令。

### 2.3 Example Compilation

Suppose we wish to compile this fragment of code into assembly:

height = (width+56) * factor(foo);

The first stage of the compiler (the scanner) will read in the text of
the source code character by character, identify the boundaries between
symbols, and emit a series of **tokens**. Each token is a small data structure
that describes the nature and contents of each symbol:

假设我们希望将以下代码段编译为汇编形式：
```
height = (width+56) * factor(foo);
```
编译器的第一步（扫描器）会逐字符地读取源代码，区分每个符号，并产生一系列的句柄。
每个句柄都能描述其所代表的符号的内容及属性。

```
id:height = ( id:width + int:56 ) * id:factor ( id:foo ) ;
```
At this stage, the purpose of each token is not yet clear. For example,
factorandfooare simply known to be identifiers, even though one is
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

