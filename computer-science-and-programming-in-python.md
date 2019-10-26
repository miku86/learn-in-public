# Introduction to Computer Science and Programming in Python

Source: https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/

## Computation

### Overview

- computer performs calculations
- calculations are built-in or defined by developer
- computer only knows what you tell it
- types of knowledge:
  - declarative: statements of facts
  - imperative: recipe how to do sth
- recipe (= algorithm)
  1. sequence of (simple) steps
  1. flow of control that specifies when each step is executed
  1. a means of determining when to stop
- fixed program computer: calculator
- stored program computer: machine stores and executes instructions
- basic machine architecture:
  - input
  - memory
  - cpu (control unit + arithmetic logic unit)
  - output
- sequence of instructions stored inside computer
  - built from predefined set of primite instructions:
    - logic and arithmetic
    - tests
    - moving data
- interpreter executes each instruction in order
- turing: you can compute anything using 6 primitives, anything computable in one language is also doable in any other language
- a language provides a set of primitive operations
- primitives:
  - english: words "boy"
  - programming: strings, numbers, booleans, simple operations
- syntax:
  - english: sentence "boy is apple" => syntax correct (noun, verb, noun), but wrong semantics
  - programming: expression
- static semantics
  - english: senctence "boy eats food" => syntax + semantics correct
- errors:
  - syntax
  - semantics
  - different meaning than what developer expected
- program is a sequence of definitions and commands:
  - definitions evaluated
  - commands executed (instruct interpreter to do sth)

### Objects in Python

- program manipulates data objects
- object has a type
- type defines the things a program can do to them (x is human, so he can speak, eat etc.)
- objects are scalar (can't get subdivided), non-scalar(has internal structure that can be accessed)
- scalar objects: int, float, bool, NoneType
- can convert (=cast) one type to another

### Expressions

- expressions are complex combinations of primitives
- expressions and computations have values and meanings
- expression = objects + operators
- every expression has a value, which has a type
- simple expression syntax: `object operator object`

### Variables

- assignment: bind variable to value
- can re-bind variables using new assignment

---

## Branching & Iteration

### Strings

- characters, letters, whitespace, digits
- `print()`
- `input()`

### Conditionals

- controls where program flows: `if else`

### Loops

- do stuff repeatedly
- `while`: while sth is true, do this
- `for`: for n times, do this

---

## String Manipulations

- strings are immutable
- length: `len`
- indexing: `s[n]`
- slice: `[::]`

## Algorithms

- guess-and-check: guess a solution and check it
- approximation: start with a guess and increment by some small value
- bisection search: half interval each iteration, `logsub2N`

---

## Abstraction, Decomposition, Functions

### Abstraction

- a TV is a blackbox
- know the interface: input and output
- input: connect other device to it that has data
- blackbox converts input to output
- Abstraction: do not need to know how TV works

=> programming: function specification , docstring
Why:

- can't see details
- don't need to see details
- don't want to see details
- hide implementation details

### Decomposition

- combine multiple TVs to display a big image
- each TV takes input and produces output
- Decomposition: different devices work together to achieve a goal

=> programming: code divided into modules => functions or classes
Why:

- self-contained
- reusable
- keep code organized
- keep code coherent

### Function

- reusable chunks of code
- have to get called/invoked
- has name, parameters, body, a return, docstring (optional, but recommended)
- scope: environment, where stuff lives
- function declarations only seen as `some code` until invoked

---
