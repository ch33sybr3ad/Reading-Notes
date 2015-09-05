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
  - **YARV’s internal stack and your Ruby stack**
    - Koichi Sasada and the Ruby core team designed YARV to use a stack pointer and a program counter, that is function like your computer's actual microprocessor. 
    - YARV keeps track of not only its own internal stack, but also your Ruby program's *call stack*, making it a "double-stack" machine. 
    - Executing Ruby with YARV using 1.9+ is 4.25 times faster than Ruby 1.8 versions and lower. Ruby 1.8 is faster with programs that run less than 11,000 iterations. 
  - **Local and Dynamic Access of Ruby Variables**
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

## Chapter 4: Control Structures and Method Dispatch
  - **YARV Contrl Structures:**
    YARV has its only control structures like Ruby, except at a much lower level
    - instead of if and unless, YARV uses branchif and branchunless
    - for looping, YARV uses jump to change the program counter and move through your compiled program.
    - method dispatch: The process where YARV uses send instructions
    when your code calls a method.
  - **How Ruby Executes an if Statement:**
    - Ruby follows a pattern when implementing the if..else statement
      - Evaluate condition
      - Jump to false code if condition is false
      - True code; jump past false code
      - False code
  - **Jumping From one Scope to Another:**
    - In Ruby you can use break to exit the current scope, but the task is a little bit harder for YARV
    - In order for YARV to know where to jump to Ruby uses the *throw* YARV instructions, which sends the execution path back up to a higher scope. 
    - Catch Tables: a table of pointers that may be attached to any YARV code snippet. 
      - while executing throw instructions, it will check the catch table to see whether it contains a break pointer for the current YARV instruction sequence. 
      - If it doesn't find a catch table, Ruby starts to iterate down through the stack of rb_control_frame_t structures in search of a catch table containing a break pointer
      - Once Ruby finds the catch table pointer, it resets both the Ruby call stack (the CFP pointer) and the internal YARV stack to reflect the new pro- gram execution point.
      - **Other Uses:** If you call return from inside a block, Ruby raises an internal exception that it rescues with a catch table pointer in the same way it does when you call break. 
        - Ruby uses the catch table to implement the control structures rescue, ensure, retry, redo, and next.
  - **the send instruction: Ruby’s Most complex control structure**
    - The send instruction tells YARV to jump to another method and start executing it.
    - YARV calls send -> Method Lookup -> Method Dispatch -> YARV executes target method
  - **Eleven Types of Ruby:**
    During the method dispatch process, Ruby determines which type of method your code is trying to call. Most important are ISEQ, CFUNC, ATTRSET, and IVAR.
    - **ISEQ:** Most of the methods are referred to as *instruction sequence* methods. This is because Ruby compiles your code into a series of YARV bytecode instructions. 
    - The other 10 methods consists of YARV's internal method types. 
      - **CFUNC:** Using C code included directly inside the Ruby executable, these are the methods that Ruby implements rather than you. CFUNC stands for C function.
      - **ATTRSET:** A method of this type is created by the attr_writer method. ATTRSET stands for attribute set.
      - **IVAR:** Ruby uses this method type when you call attr_reader. IVAR stands for instance variable.
      - **BMETHOD:** Ruby uses this method type when you call define_method and pass in a proc object. Because the method is represented internally by a proc, Ruby needs to handle this method type in a special way.
      - **ZSUPER:** Ruby uses this method type when you set a method to be public or private in a particular class or module when it was actually defined in some superclass. This method is not commonly used.
      - **UNDEF:** Ruby uses this method type internally when it needs to remove a method from a class. Also, if you remove a method using undef_method, Ruby creates a new method of the same name using the UNDEF method type.
      - **NOTIMPLEMENTED:** Like UNDEF, Ruby uses this method type to mark certain methods as not implemented. This is necessary, for example, when you run Ruby on a platform that doesn’t support a particular operating system call.
      - **OPTIMIZED:** Ruby speeds up some important methods using this type, like the Kernel#send method.
      - **MISSING:** Ruby uses this method type if you ask for a method object from a module or class using Kernel#method and the method is missing.
      - **REFINED:** Ruby uses this method type in its implementation of refinements, a new feature introduced in version 2.0.
  - **Calling Normal Ruby Methods:** 
    - VM_METHOD_ TYPE_ISEQ constant means that the ruby method contains YARV instructions. 
    - Types of Arguments: Block Arguments, Optional Arguments, Splat Arguments Array, Standard and Post Arguments
    - Calling a CFUNC method creates a new framestack just like ISEQ methods; however, Ruby uses a rb_control_frame_t structure with the type CFUNC isntead. 
    - IVAR and ATTRSET are used to speed up the process of accessing and setting instance variables in your code. Using these special methods will speed up method dispatch and make your program run faster. Because both reader and writer methods are so short, Ruby does not need to create a new stack frame. 















