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

`Parser`: Gives syntax a grammar -  the ability to compose larger expressions and statements into smaller parts.  A **parser** takes flat sequence of tokens and builds a tree which mirrors the nature of the grammer. These are called **parse trees** or **Abstract Syntax Trees (AST)**. It also tells us if code is valid and if it doesnt it generates syntax errors. 

`Static analysis`: It adds further meaning to the AST. for eg `a+b` is addition of a and b but what are a and b. The first step is **binding** or **resolution** which finds the name and binds it to a value where it is defined. Then **scope** comes to play to limit where an identifier is accessed. If language is statically typed then we do a type check and for errors we report **type errors**

### Intermediate Representation
#todo 

### Backend
#todo 

## Scanning
#todo

## Representation of code
Grammar of language is defined through **context free grammar (CFG)**.  
`A --> alpha`  
Where A is non terminal symbol (head) and alpha is a string of terminals and/or non terminals (body).  
A can always be replaced by alpha.  
- A **terminal** is a letter from grammers alphabet. Its like a literal value. In parser, it would be individual tokens.  
- A **non terminal** is a named reference to another rule in the grammer. We can replace that rule with any further terminal or non terminal and keep playing further.

A formal grammer takes in a set of atomic pieces called the 'alphabet' . It then defines a set of 'strings' (usually infinite possibilities) that are in the grammer. Each string consists of a sequence of letters in the alphabet.  
A formal grammers job is to specify which strings are valid and which are not.  

| Terminology    | Lexical Grammar | Syntactic Grammar |
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

## Parser
Parser uses the grammar rules to create an AST which represents the expression as a recursive tree. The parser uses different techniques and the book uses **Recursive Descent Parsing** or a top-down parser. Some other techniques are: LL, LR, LALR, parser combinators, Earley parsers, the shunting yard algorithm, packrat parsing etc.  

The expressions in code can be ambiguous and if they are, parser uses operator precedence to determine which to group first (they go down in the tree) and which to group later (they are up in the tree)

### Syntax Errors during Parsing
If given a valid sequence of tokens then parser produces the syntax tree.  
If given an invalid sequence of tokens then it should detect and tell the user about their mistakes.  

Hard requirements:
- Detect and report the error: If malformed tree is passed further then the interpreter breaks.
- Avoid crashing or hanging: Syntax  errors are common. Crashing or getting stuck in infinite loop isnt allowed. Even if source code is not valid, the input to the interpreter is a valid one.  

Decency requirements:
- **Be fast**: People expect their editors to reparse files in milliseconds and computers are fast enough to do it.  
- **Report distinct errors**: Return multiple and dont stop at one error since that would be really annoying.
- **Minimize cascade errors**: Once a single error is found the next few tokens would also likely be invalid, parser cant deterministically tell what the original intention was. It might report ghost errors which make it seem that the program is in a worst state than it is.  

Distinct errors and minimizing cascade errors is at odds: we should report as many errors as we can but also try not to report ones that are sideeffects caused by an earlier error.  
**Error recovery**: The way a parser responds to the error and keeps going to report later errors.  
One of the recovery techniques is called **panic mode**. When the parser detects an error, it enters panic mode, we know at least one token doesnt make sense.  
Before continuing parsing, state and sequence of next tokens need to be aligned. This is called **synchronization**. We select a rule in grammar to mark a synchronization point, parser fixes state by jumping out of nested grammar rules till the start of the rule and syncs by discarding tokens until it reaches one of the sync points in the code.  
This does mean that real syntax error in discarded tokens are missed, but also side effects are not falsely reported so good trade off.  
Traditionally synced between statements.  