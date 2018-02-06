# Introduction to functional programming

## Why

### People will miss the point

- Multicore blabla
- Async non-blocking blabla
- Reactive blabla

This is not the heart of the matter. It is either misunderstanding and / or marketing pitches.

### The benefit of functional programming

> Functional programming increases programmer productivity and program correctness
through **modularity** and **composition**

#### Modularity

> Modularity is the extent to which I can reason about a program by reasoning
about its parts

It lets me break things apart.

#### Composition

> Composition is the extent to which I can build useful new programs out of existing programs,
and these new programs behave as expected.

It lets me put things together.

## What is functional programming

> Functional programming is programming with functions

### How is this useful?

Functions can be broken apart and put back together. Functions are a modular programming abstraction,
and lend themselves to composition, by definition.

###  Function

It is a function in the mathematical sense.

`f: A -> B`

- Total: each element in `A` is mapped to some element in `B`
- Deterministic: Whenever I pass in some `a ∈ A`, i get the same `b ∈ B` back
- Completely determined by the input and output - there are no (externally visible)
effects besides the return value

### Functions have types

Without types, a function cannot be unambiguously defined.

### Function composition

Given `f: A -> B` and `g: B -> C`, I can always construct a unique function `h : A -> C`
such that `h(x) = g(f(x))`.

### Functions have types

We need types to define function composition.

We can only compose functions if the input of the second function is the same as the output
of the first function.


### Examples of non-functions

<<demo>>

### Teaser - Examples of side effects and dealing with them

<<demo>>

## Exercises

*Test-drive all the excercises.*

- Implement a function `compose` which composes two functions, as described above
- Implement a function `curry` which takes a  function of two arguments,
` (A, B) -> C` and returns
a function of one argument, `A -> (B -> C)`. That is, a function which takes an `A`
and returns another function of type `B -> C`
- Implement the function `identity` of type `A -> A`.
- Implement the function `const`, of type `B -> A -> B`
- How many possible implementations does each of the above functions have?




