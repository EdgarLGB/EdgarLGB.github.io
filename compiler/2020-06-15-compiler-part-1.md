---
layout: default
title: Compiler part 1
parent: Compiler
nav_order: 1
---
## What's up?
I hope everyone stays safe. 2020 is not a usual year when everything seems stuck.
I have been locked down at home for more than three months meaning no need to spend time going back
and forth between home and the office which is good. So I tried to learn something interesting during this time.
What inspired me is the work I did in Apache SystemML during [GSoC 2018](http://edgarlgb.github.io/Gsoc-project/). The work was that I implemented a
built-in function from compilation to runtime. So I would like to know how does a program work under-the-hood.

## How a program works?
A program is merely a text file but how do we make the computer understand what it is asked to do. Nowadays, we usually write a program in a high-level programming
language for example Java which is only understandable to human. And the computer only understands the machinery code. We also have some prepared instruction sets which allow to order the computer
to do what you want. But we are not writing the program using that complex low-level instructions (which sounds impossible to write an app using it), then how do we translate the high-level programming language to those instructions? 
We need a compiler. A compiler is a system to translate a high-level programming language into a low-level one such as the intermediate instructions. 
While an interpreter is a system to execute the instructions upon an operating system. Take Java as an example, javac is a compiler which translates the class
files into object byte codes and then the JVM (Java Virtual Machine) will read those intermediate instructions and execute it. It is not surprising that people 
sometimes call the interpreter a virtual machine. In this tutorial, I will only focus on the compiler.

## Architecture
In general, the architecture of a compiler consists of two parts: frontend and backend. The frontend is a process of turning a stream of characters into a set of intermediates 
including lexer, syntax analysis, semantic analysis, generation of intermediates. The backend refers to the runtime, the execution part.

## What is the task of tokenization?
The task is to read a stream of characters and generate a stream of tokens. A token is the minimum unit of a language which can't be split up, for example, an integer, a keyword.

## Implementation of tokenization
The idea is very straightforward. It takes an input of a stream of characters and outputs a stream of tokens. The key point of the implementation is how to determine the
token based on the current character. That means when it meets an alphabet, it can uniquely decide this is going to be an identifier (keyword is also an identifier). When 
it reads a digit, it can determine this is going to be a number . So I cite some of the rules defined in our language using regular expression: (the left part infers
the right parts)

* number -> Integer optionalPoint optionalFraction
* Integer -> ([0-9])+
* optionalPoint -> . | e (empty)
* optionalFraction -> ([0-9])+
* identifier -> ([a-Z])([a-Z]|[0-9])*
* relationOperator  ->  > | < | >= | <= | == | !=
* operator -> + | - | * | / | %

### Read

```java
public class Lexer {

    private final InputStream inputStream;
    private char getChar() throws IOException {
        return (char) inputStream.read();
    }

}
```

### Scan an identifier
Here is an example showing how to grab an identifier token. A valid identifier includes either alphabets or digits but always starts with an alphabet.
The idea is to read the characters one by one from the input stream and to see it starts with an alphabet, if so that means we meet an identifier. And then 
we continue to read the characters until we encounter a character which is neither alphabet nor digit. As you look into the code, you might wonder what is 
a lexeme. A lexeme is like a word of a language which shows exactly what characters is this token made of. Another question is that we want to create a token only once even
if we met the same lexeme more than once. We can create an identifier table to keep all the deja-vu identifiers. And then you might ask
how do we differentiate an identifier from a keyword given that they are all made of either characters or digits? In fact, a keyword is a token that we preserve to be used
internally by the language meaning the users can't define an identifier with the same lexeme of a keyword. We can't define **true** as an identifier since it has
been preserved to be a keyword. To know if the current token is a keyword, we previously insert the keyword tokens into **identifierMap** which is a map of
_lexeme_ => _token_. When we have a lexeme, we look it up in this map to see if there is a corresponding token.

```java
public class Lexer {

    private final InputStream inputStream;
    private char current;
    private final Map<String, Identifier> identifierMap;  // the mapping of lexeme -> <tag, lexeme>

    public Lexer(InputStream input) {
        inputStream = input;
        current = '\0';
        lookahead = '\0';
        identifierMap = new HashMap<>();
        addKeywords(identifierMap);
    }

    private void addKeywords(Map<String, Identifier> identifierMap) {
        identifierMap.put("true", new Identifier(Tags.TRUE, "true"));
        identifierMap.put("false", new Identifier(Tags.FALSE, "false"));
    }

    private char getChar() throws IOException {
        return (char) inputStream.read();
    }
    
    private Token scan() {
        Token res;
        if (isCharacter(current)) {  // read identifier token (variables, keywords)
            res = scanIdentifier();
        }
        return res;
    }

    private Token scanIdentifier() throws Exception {
        StringBuilder sb = new StringBuilder();
        do {  // Currently only support the identifiers made of characters and digits (but always starts with character)
            sb.append(current);
            match(current);
        } while (isCharacter(current) || isDigit(current));
        register = current;

        String lexeme = sb.toString();
        Identifier id = identifierMap.get(lexeme);
        if (id == null) {
            id = new Identifier(Tags.ID, lexeme);
            identifierMap.put(lexeme, id);
        }
        return id;
    }
}
```

## Next part
In the next part, I will talk about the grammar of a language, such as the context, terminators, rules, etc.
