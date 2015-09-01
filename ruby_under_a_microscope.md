# Ruby Under a Microscope <br/> by Pat Shaughnessy - 2014

Start: August 25th, 2015 <br/>
Finished: 

## Chapter 1: Tokenization and Parsing
  - Whenever you run a Ruby script, Ruby will read and transform your code *three* times before running it.
    - Ruby Code -> Tokenize -> Parse -> Compile -> YARV Insctructions
    - Tokenize: converts text characters into tokens
    - Parse: groups tokens into meaningful Ruby statements
    - Compile: statements are converted into low-level intructions that it can execute later using a virtual machine. 
  - **Tokens: the words that Make up the Ruby language**
    -The Ruby C source code contains a loop that reads in one character at a time and processes it based on what that character is. When Ruby sees these characters, it tokenizes them. That is, it con- verts them into a series of tokens or words that it understands by stepping through the characters one at a time. 
    - Reserved words are keywords that carry significant meaning in Ruby because they provide the structure, or framework, of the language, i.e. alias, and, begin, break, case, class.
  - **Parsing: How Ruby understands your code**
    - Parsing is where words or tokens are grouped into sentences or phrases that make sense to Ruby. Ruby uses a parser to process tokens, but the parser itself is generated with a parser generator. Parser generators take a series of grammar rules as input that describe the expected order and patterns in which the tokens will appear.
      - Ruby uses a newer version of Yacc called Bison. 
      - The parse.y file defines the actual syntax and grammar that you have to use while writing your Ruby code
      - Ruby runs Bison ahead of time during the build process to create the actual parser code. 
    - LALR, or Look-Ahead Left Reversed Rightmost Derivation, is the algorithm used by the parser to process incoming tokens. 
    - As Ruby parses your code, matching one grammar rule after another, it converts the tokens in your code file into a complex internal data structure called an abstract syntax tree (AST). 

## Chapter 2: Compilation
  - The word compile means to translate your code from one programming language to another. C is combiled to machine language, Java into Java Virtual Machine. Ruby's compiler translates into Ruby's virtual machine and runs automatically. 
  - Ruby was first introduced with a compiler in Ruby 1.9. Ruby 1.8 and below immediately executes your code after the tokenizing and parsing processes are finished.
  - **YARV:** 
    - Yet Another Ruby Virtual Machine, the compiler implemented with Ruby 1.9, similar to JVM. You first compile your code into bytecode, a series of low-level instructions that the virtual machine under- stands. However, Ruby never exposes its compiler as an external tool and never compiles your Ruby code all the way into machine language. 
    - Ruby -> Tokens -> AST Nodes -> YARV instructions ----> C -> Machine Language
    - YARV is a stack-orientated virtual machine, it maintains a stack of values—mainly arguments and return values for the YARV instructions.
  - Ruby compiles each distinct scope in your Ruby program—methods, blocks, classes, or modules, for example into a separate snippet of YARV instructions.
  - Each snippet of YARV instructions, each scope in your Ruby program, has its own local table.
  - Keyword Arguments: we can specify a name along with a default value for each method or block argument.

## Chapter 3: How Ruby Executes Your Code
  -**YARV’s internal stack and your Ruby stack**
    - Koichi Sasada and the Ruby core team designed YARV to use a stack pointer and a program counter, that is function like your computer's actual microprocessor. 
    - YARV keeps track of not only its own internal stack, but also your Ruby program's *call stack*, making it a "double-stack" machine. 
    - Executing Ruby with YARV using 1.9+ is 4.25 times faster than Ruby 1.8 versions and lower. Ruby 1.8 is faster with programs that run less than 11,000 iterations. 
  -**Local and Dynamic Access of Ruby Variables**
    - Ruby stores all of the values you save in variables on YARV’s stack, along with the parameters to and return values from the YARV instructions.
    - Ruby uses two very different methods for saving and retrieving a value you save in a variable: local access and dynamic access.
    - **Local Access**
      - Ruby sets aside some space on YARV stack for any local variables when making a method call. 
      - Ruby uses svar/cref to contain one of two things: either a pointer to a table of the special variables in the current method or to the current lexical scope. 
      - YARV uses another pointer similar to the stack pointer, called the EP or environment pointer, to write in a new value for a variable when reassigned. 
    - **Dynamic Variable Access**
      - Ruby uses dynamic access when you use a variable that’s defined in a different scope
      - Ruby uses dynamic access in order to obtain values further down the stack while executing code inside the block. 
        - the EP pointers form a kind of ladder that Ruby can climb to access the local variables in the parent scope, the grandparent scope, and so on.
        - Ruby uses the special variable at each level of the stack to preserve the value of your variable in different scopes.













