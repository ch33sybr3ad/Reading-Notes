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






