# Laziness 

## Java is strict

A strict function is a function whose arguments need to be evaluated, before the function itself is
evaluated. In the expression

```java
    sum(foo(), bar())
```
, `foo()` and `bar()` need to be evaluated before `sum(4, 6)` is evaluated.


> The above definition is not completely correct. We will use 'non-strict' and 'lazy' interchangeably here,
which will suffice for our purposes. See https://wiki.haskell.org/Lazy_vs._non-strict and 
https://en.wikipedia.org/wiki/Strict_function for a more formal treatment.


That's how Java works, right? 

## Java is not always strict

Java functions / expressions are strict, **except in some control flow structures and builtin operators**


- Consider
```java
boolean x = y() || z();
```
- In case `y()` evaluates to true, `z()` will not be evaluated
- This has practical implications - *non-terminating expressions*

## Emulating non-strictness

- We want to have non-strict functions
    - Userland control flow functions
        - try with resources for things that don't implement closeable
        - transact()
        - Many more
    - Infinite data structures
    - **Delaying side effects**

- We can emulate a lazy expression through a function that takes no input
    - `fj.F0`
    - Userland `if-then-else`
    - Userland `try-with-resources`
    
## Transformations on non-strict values

- `F0` on steroids
- `P.lazy()`
- Transform a lazy value to another value, without forcing its evaluation
    - `map`, `bind`

Transforming and chaining side effects without forcing their evaluation is very useful.
We will see it in action when looking at `fj.control.DB`





