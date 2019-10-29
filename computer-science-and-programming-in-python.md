# Introduction to Computer Science and Programming in Python

Source: https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/

## 01. Computation

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

## 02. Branching & Iteration

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

## 03. String Manipulations

### Basics

- strings are immutable
- length: `len`
- indexing: `s[n]`
- slice: `[::]`

### Algorithms

- guess-and-check: guess a solution and check it
- approximation: start with a guess and increment by some small value
- bisection search: half interval each iteration, `logsub2N`

---

## 04. Abstraction, Decomposition, Functions

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

## 05. Tuples, Lists, Aliasing, Mutability, Cloning

### Compound Data Types

- made up of other data types

### Tuples

- ordered sequence of (mixed) elements
- immutable
- creation: `()`
- swap: `(a, b) = (b, a)`
- iterable

### Lists

- ordered sequence of (mixed) elements
- mutable
- creation: `[]`
- iterable
- add element: `a.append(b)`
- concat lists: `a.extend(b)`
- delete element: `del(a[index])`
- sort list: `a.sort()`
- reverse list: `a.reverse()`
- string to lists and vice-versa: `split()`, `join()`

### Aliasing, Mutability, Cloning

- variable name points to object, e.g. a person
- many different variables can point to the same object, e.g. person has multiple nicknames
- if you change the object (=> the person object), any variable (=> any nickname) pointing to that object is affected

=> side effects!

- copy list: `copy = original[:]`

---

## 06. Recursion, Dictionaries

### Recursion

- the process of repeating items in a self-similar way
- algorithmically: a way to design solutions by divide-and-conquer: reduce a problem to simpler versions of the same problem
- semantically: a programming technique where a function calls itself: `base case + recursive step`
- e.g. `multiplication of a and b` by `adding a to itself b times`
- each recursive call to a function creates its own scope
- bindings of variables in a scope are not changed by recursive call
- flow of control passes back to previous scope once function call returns value

### Iteration vs. Recursion

```py
# iteration
def factorial_iter(n):
  prod = 1
  for i in range(1, n+1):
    prod *= i
  return prod

# recursion
def factorial(n):
  if n == 1:
    return 1
  else:
    return n * factorial(n-1)
```

- recursion may be simpler, more intuitive
- recursion may be efficient from programmer point of view
- recursion may not be efficient from computer point of view

### Induction

- To prove a statement indexed on integers is true for all values of n:
  - Prove it is true when n is smallest value (e.g. n = 0 or n = 1)
  - Then prove that if it is true for an arbitrary value of n, one can show that it must be true for n+1

### Dictionaries

- store data as key-value pairs
- creation: `{}`
- unordered
- add element: `dict[newKey] = newValue`
- delete element: `del(dict[key])`
- get all keys: `dict.keys()`
- get all values: `dict.values()`

### List vs. Dictionary

- List: ordered <===> Dict: unordered
- List: matches index to value <===> Dict: matches key to value
- List: look up element by index (starts from 0, increasing) <===> Dict: look up element by key (whatever you like)

---

## 07. Testing, Debugging, Exceptions, Assertions

- You are making soup but bugs keep falling in from the ceiling. What do you do?
  - keep lid closed: defensive programming
  - check soup for bugs: testing
  - clean kitchen: debugging

### Defensive Programming

- Decomposition
- Abstraction

### Testing

- Compare input/output pairs to specification

#### Types

- Unit Test: validate each piece of program separately
- Regression Test: catch reintroduced errors that were previously fixed
- Integration Test: test if overall programm works

#### Approaches

- black box testing:
  - explore paths through specification
  - can be reused if implementation changes
  - can be done by someone other than the implementer (biased)
- glass box testing:
  - explore paths through code
  -  called path-complete if every potential path through code is tested at least once

### Debugging

- Study events leading up to an error
- steep learning curve
- goal is to have a bug-free program
- tools:
  - print with bisecting
  - read error messages
  - logic errors: think before coding, explain code to someone else

### DON'T

1. write entire program
2. test entire program
3. debug entire program

### DO

1. write one function
2. unit test this function
3. debug this function
4. integration test
5. repeat from 1.

### Exception

- handle exception: use try & except
- never fail silently
- instead raise exception

### Assertions

- assertions don’t allow a programmer to control response to unexpected conditions
- ensure that execution halts whenever an expected condition is not met
- typically used to check inputs to functions, but can be used anywhere
- can be used to check outputs of a function to avoid propagating bad values
- can make it easier to locate a source of a bug
- goal is to spot bugs as soon as introduced and make clear where they happened
- use as a supplement to testing
- raise exceptions if users supplies bad data input
- use assertions to:
  - check types of arguments or values
  - check that invariants on data structures are met
  - check constraints on return values
  - check for violations of constraints on procedure (e.g. no duplicates in a list)

---
