# List folds

## Common pattern

- Implement `sum`
    - `0` if empty
    - `x + sum(xs)` if `x:xs`
- product
    - `1` if empty
    - `x * product(xs)` if `x:xs`
    
The above functions are essentially the same. They only differ in the binary operation (`+`, `*`)
and the 'initial element' (`0`, `1`).

We also have this repetition in our homework. We hand-code recursion everywhere!
- Let's extract it in functions.


## Fold left

Encapsulates a loop over a list. Given a `List<A>`
- Start with an initial value `b: B` to hold our result
- Apply, at each element of the list, a function `(B, A) -> B` to produce the next result from the
previous result and the current list element
- Until empty

### `sum` and `product` again

- Let's restate `sum` and `product` in terms of `foldLeft`.
- Let's write `length` via `foldLeft`

### Tracing how foldLeft evaluates

`foldLeft` is parametric on its return type, `B`. We can substitute `B` for `List<A>` and get back a list.

```java
xs.foldLeft((xs, x) -> cons(x, xs), empty())
```

- Use `f=cons`
- Use `z=empty()`

What do we get back?

- `foldLeft(1:2:3, f, z)`
- `nil`
- `1:nil`
- `2:1:nil`
- `3:2:1:nil`

We got `xs.reverse()` back.

## Fold right

```java
public <B> B foldRight(BiFunction<A, B, B> f, B z)
{
    return isEmpty() ? z : f.apply(head(), tail().foldRight(f, z));
}
```
Encapsulates
- Applying a function `f` to each `:`
- Replacing `empty()` with `z`

Let's trace `xs.foldRight((x, acc) -> acc +1, 0)`
- `1 + (fr(2:3:4))`
- `1 + (2 + (fr(3:4)))`
- `1 + (2 + (3 + (fr(4))))`
- `1 + (2 + (3 + 4 + fr(empty())))`
- `1 + (2 + (3 + (4 + (0))))`
- `1 + (2 + (3 + (4)))`
- `1 + (2 + (7))`
- `1 + (9)`
- `10`



### Identity

`foldRight` just replaces each `:` with `f`. You can think of it as a purely syntactic translation
of list structure.

What does this give?

```java
xs.foldRight((x, xxs) -> cons(x, xxs), empty())
```

### Append a list to another list

We can do the above exercise, letting `z=<another list>` instead of `empty()`

```java
xs.foldRight((x, xxs) -> cons(x, xxs), ys)
```


## Exercises

- You already know the `map` function. Implement it via a fold
- Implement via a fold the function `forAll`, which returns true if a predicate is satisfied by all elements in the list,
false otherwise
- Implement via a fold the function `exists`, which returns true if a predicate is satisfied by any element,
false otherwise
- You already know the function `filter`. Implement it via a fold.
- Implement a function `foldRightC`. It behaves like `foldRight`, but is implemented in terms of `foldLeft`
