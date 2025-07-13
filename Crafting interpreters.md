## Intro Concepts
`Self Hosting`: A compiler reads files in one language, translates them, and outputs files in another language. You can implement a compiler in any language, including the same language it compiles, a process called **self-hosting**.

`Bootstrapping`: Once a compiler in another language is made to compile to your language, then the compiled version can be used to develop future versions of the compiler in your own language and the original can be discarded.

### Frontend
This process is moving from source code to a representation of code as state with meaning.

`Scanning / Lexing / Lexical analysis`: Takes a linear stream of characters and groups them together into series of tokens which are like words.
```
var average = ( min + max ) / 2;
// turns to var, average, =, (, min, +, max, ), /, 2, ;
```

`Parser`: Gives syntax a grammer -  the ability to compose larger expressions and statements into smaller parts.  A **parser** takes flat sequence of tokens and builds a tree which mirrors the nature of the grammer. These are called **parse trees** or **Abstract Syntax Trees (AST)**. It also tells us if code is valid and if it doesnt it generates syntax errors. 

`Static analysis`: It adds further meaning to the AST. for eg `a+b` is addition of a and b but what are a and b. The first step is **binding** or **resolution** which finds the name and binds it to a value where it is defined. Then **scope** comes to play to limit where an identifier is accessed. If language is statically typed then we do a type check and for errors we report **type errors**

### Intermediate Representation
#todo 

### Backend
#todo 

## Scanning
#todo

## Representation of code
Grammer of language is defined through **context free grammer (CFG)**.  
`A --> alpha`  
Where A is non terminal symbol (head) and alpha is a string of terminals and/or non terminals (body).  
A can always be replaced by alpha.  
- A **terminal** is a letter from grammers alphabet. Its like a literal value. In parser, it would be individual tokens.  
- A **non terminal** is a named reference to another rule in the grammer. We can replace that rule with any further terminal or non terminal and keep playing further.

A formal grammer takes in a set of atomic pieces called the 'alphabet' . It then defines a set of 'strings' (usually infinite possibilities) that are in the grammer. Each string consists of a sequence of letters in the alphabet.  
A formal grammers job is to specify which strings are valid and which are not.  

| Terminology    | Lexical Grammer | Syntactic Grammer |
| -------------- | --------------- | ----------------- |
| 'alphabet' is  | Characters      | Tokens            |
| 'string' is    | Lexeme or Token | Expressions       |
| implemented by | Scanner/Lexer   | Parser            |


**Backus-Naur Form (BNF)** one form of reprentation introduced with ALGOL 58 and now most languages use a version of that.  

```
expression     → literal
               | unary
               | binary
               | grouping ;

literal        → NUMBER | STRING | "true" | "false" | "nil" ;
grouping       → "(" expression ")" ;
unary          → ( "-" | "!" ) expression ;
binary         → expression operator expression ;
operator       → "==" | "!=" | "<" | "<=" | ">" | ">="
               | "+"  | "-"  | "*" | "/" ;
```