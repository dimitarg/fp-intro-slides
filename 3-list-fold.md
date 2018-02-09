# List folds

- sum
- product
- foldLeft
- sum and product again
- length
- foldLeft with cons and nil equals what ? 
    - trace evaluation of foldLeft
- foldLeft with cons and another list
- foldRight
- identity
- append



## Exercises

- You already know the `map` function. Implement it via a fold
- Implement via a fold the function `forAll`, which returns true if a predicate is satisfied by all elements in the list,
false otherwise
- Implement via a fold the function `exists`, which returns true if a predicate is satisfied by any element,
false otherwise
- You already know the function `filter`. Implement it via a fold.
- Implement a function `foldRightC`. It behaves exactly like `foldRight`, but is implemented in terms of `foldLeft`