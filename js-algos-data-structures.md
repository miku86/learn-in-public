# JavaScript Algorithms & Data Structures

## Big-O Notation

### What is Big-O?

- algorithm: a set of steps to do a certain task
- data structure: how data is structured
- allows us to talk formally about how the runtime or needed memory of an algorithm grows as the inputs grow
- to classify the performance of an algorithm (in time or space)
- depends only on the algorithm, not the hardware

Possibilities:

- f(n) could be constant: `O(1)`
- f(n) could be linear: `O(n)`
- f(n) could be quadratic: `O(n^2)`
- f(n) could be logarithmic: `O(log n)`

Simplifying Big O Expression:

- constants don't matter: `O(100) => O(1)`, `O(2n) => O(n)`, `O(5n^2) => O(n^2)`
- smaller terms don't matter: `O(n + 50) => O(n)`, `O(10n + 500) => O(n)`, `O(n^2 + 3n + 10) => O(n^2)`

### Time Complexity

Shorthands:

- arithmetic operations are constant
- variable assignment operations are constant
- accessing elements in an array or object is constant
- in a loop: length of the loop x complexity of what happens inside the loop

### Space Complexity (space of the input is not included)

Shorthands:

- strings require `O(n)` space (n = length of string)
- the other primitives are constant space
- arrays & objects require `O(n)` space (n = length of array or amount of keys)

## Default Data Structures

### Objects

- use them: when you do not need order, when you need fast read, create, delete
- have a look at Hash Tables and Hash Maps
- Read: `O(1)`
- Create: `O(1)`
- Delete: `O(1)`
- `Object.keys()`: `O(n)`
- `Object.values()`: `O(n)`
- `Object.entries()`: `O(n)`
- `hasOwnProperty()`: `O(1)`

### Arrays

- use them: when you do need order, when you need fast read, create, delete
- Read: `O(1)`
- Create: depends on the index: the lower the index, the more other elements we have to re-index
- Delete: depends on the index: the lower the index, the more other elements we have to re-index
- Search: `O(n)`
- `push()`: `O(1)`
- `pop()`: `O(1)`
- `shift()`: `O(n)`
- `unshift()`: `O(n)`
- `concat()`: `O(n)`
- `slice()`: `O(n)`
- `splice()`: `O(n)`
- `sort()`: `O(n log n)`
- `forEach, map, filter, reduce()`: `O(n)`

## Problem Solving Approach

### 1. Understand the problem:

- Restate the problem in your own words
- What are the inputs?
- What are the outputs?
- Can the inputs determine the outputs?
- How should I label the important pieces?

### 2. Explore concrete examples

### 3. Break it down

### 4. Solve/Simplify

### 5. Look back and refactor

## Problem Solving Patterns
