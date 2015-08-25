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
    -
