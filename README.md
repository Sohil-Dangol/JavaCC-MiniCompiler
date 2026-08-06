# TPTP Language Interpreter

A complete **lexer, parser, semantic analyser, and interpreter** for the **Tiny Planar TelePorting (TPTP)** language, implemented in **Java** using **JavaCC**.

The interpreter parses and executes programs written in TPTP, a domain-specific language (DSL) that controls the movement of an imaginary robot across a two-dimensional plane. The project demonstrates the complete compiler pipeline from lexical analysis through to program execution, including robust semantic validation and informative error reporting. :contentReference[oaicite:0]{index=0}

---

## Features

- Lexer implemented using JavaCC token specifications
- Recursive-descent parser generated from a context-free grammar
- Abstract Syntax Tree (AST) representation
- Semantic analysis beyond grammar validation
- Interpreter for TPTP programs
- Infinite loop detection
- Detection of non-simple programs
- Custom lexical, syntax, and semantic error messages
- Expression parsing with operator precedence and associativity

---

## The TPTP Language

A TPTP program consists of:

- One or more **step** instructions
- A single **run** instruction

Each step contains:

- A unique step identifier
- Two coordinate variables
- A conditional expression
- True and false execution branches
- Optional coordinate transformations
- The next step to execute

Execution begins from the `run` instruction and continues until:

- an undefined step is reached,
- an infinite loop is detected, or
- a non-simple instruction is identified. :contentReference[oaicite:1]{index=1} :contentReference[oaicite:2]{index=2}

---

## Example Program

```text
Countdown:
if x<10
(x,y)
halt
else
(x-10,y) and Countdown;

run Countdown (2025,0)
```

Output:

```text
Success
Simple
halt 5 0
```

---

## Compiler Pipeline

```
Source Program
      │
      ▼
Lexical Analysis
(JavaCC Lexer)
      │
      ▼
Parsing
(JavaCC Parser)
      │
      ▼
AST Construction
      │
      ▼
Semantic Analysis
      │
      ▼
Interpreter
      │
      ▼
Program Output
```

---

## Expression Support

The interpreter supports arithmetic expressions consisting of:

- Integer literals
- Variables
- Addition (`+`)
- Subtraction (`-`)
- Multiplication (`*`)
- Unary negation
- Parenthesised expressions

Operator precedence is implemented correctly:

1. Parentheses
2. Multiplication
3. Addition and subtraction (left-associative)

---

## Semantic Analysis

Beyond recognising valid syntax, the interpreter validates a number of semantic constraints required by the TPTP specification, including:

- Duplicate step definitions
- Duplicate parameter names
- Undefined entry point
- Invalid condition variables
- Positive linear arithmetic restrictions
- Detection of the first non-simple instruction

Programs violating any of these constraints are rejected with descriptive error messages before execution begins. :contentReference[oaicite:3]{index=3}

---

## Interpreter

After successful parsing and semantic validation, the interpreter executes the program by:

1. Reading the starting step from the `run` instruction.
2. Binding the robot's current coordinates to the step parameters.
3. Evaluating the conditional expression.
4. Selecting the appropriate execution branch.
5. Updating the robot's coordinates.
6. Moving to the next step.
7. Repeating until termination or loop detection.

Visited program states (step name and current coordinates) are stored to detect repeated configurations, allowing infinite loops to be identified efficiently. :contentReference[oaicite:4]{index=4}

---

## Error Handling

The interpreter distinguishes between three categories of errors.

### Lexical Errors

Invalid characters are rejected during tokenisation.

Example:

```text
Failure
Line 1
Lexical error: invalid character in input.
```

### Syntax Errors

Unexpected or missing language constructs are reported with informative messages.

Example:

```text
Failure
Line 5
Syntax error: expected identifier.
```

### Semantic Errors

Language rules that cannot be expressed using grammar alone are checked after parsing.

Examples include:

- Duplicate step names
- Invalid parameter declarations
- Undefined starting step
- Invalid condition variables

---

## Project Structure

```
.
├── assignment.jj          # JavaCC grammar
├── TPTP.java              # Generated parser
├── Token.java
├── ParseException.java
├── TokenMgrError.java
├── examples/
│   ├── countdown.tptp
│   ├── loop.tptp
│   └── nonsimple.tptp
└── README.md
```

---

## Building

Generate the parser:

```bash
javacc assignment.jj
```

Compile:

```bash
javac *.java
```

Run:

```bash
java TPTP < input.txt
```

---

## Example Programs

### Successful Execution

```text
Countdown:
if x<10
(x,y)
halt
else
(x-10,y) and Countdown;

run Countdown (2025,0)
```

Output:

```text
Success
Simple
halt 5 0
```

### Infinite Loop

```text
A:
if x<1
(x,y)
becomes (1,2) and A
else
(3,4) and A;

run A (0,0)
```

Output:

```text
Success
Simple
Loop
```

### Non-Simple Instruction

```text
m:
if x<5
(x,y)
becomes (x+1,y*2) and m
else
RET1;

run m (1,2)
```

Output:

```text
Success
Non-simple
m
```

These example programs demonstrate successful execution, loop detection, and semantic analysis of non-simple instructions. They are adapted from the language specification and additional interpreter test cases used during development. :contentReference[oaicite:5]{index=5} :contentReference[oaicite:6]{index=6}

---

## Technologies Used

- Java
- JavaCC
- Recursive Descent Parsing
- Abstract Syntax Trees (AST)

---

## Learning Outcomes

This project provided practical experience with:

- Compiler construction
- Lexical analysis
- Context-free grammars
- Parser generation
- Abstract syntax trees
- Semantic analysis
- Interpreter implementation
- Recursive algorithms
- Object-oriented software design
- Robust error handling

---

## Author

**Sohil Dangol**

Computer Science Student  
University of Warwick
