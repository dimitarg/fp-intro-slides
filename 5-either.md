# Either

We will talk about Either and Validation data types.

## Disjoint union

An `Either<A,B>` models a disjoint union:

![disjoint union](assets/disjoint.png)

It is used to model a situation where something can be one of two types.

- `Int | String`
- `BasketItem | OrderItem`
- `Exception | A`

We'll get to the last one later.

## Crash course

- Construct an either - `Either.left(x)`, `Either.right(x)`
- `bimap` - transform each side of the `Either`, giving back a new `Either`
    - Either is a **bifunctor**
- Transform an `Either<A,B>` to a value of type `C`, given `f: A->B` and `g: A->C`
    - The catamorphism for eithers
- Left / Right projection
    - Apply transformations to the left / right side of an either
    - taking a value back from an either
        - value
        - orValue
        
## Using Either for error handling

### A not so successful attempt

- Read two numbers from string arguments, e.g. console
    - If either arg is null or empty, fail
    - If either arg is not a integer, fail
- Sum the integers
- If the sum is not even, fail
- Otherwise, perform whole-number division by 2

#### Problem
- Calls to `right()` everywhere
- We need a either which knows we are interested in `right()`
- Enter `Validation`

### Refactored code
 - Just change `Either<A, B>` to `Validation<A,B>`
 - Remove `right()` calls
 
## Validation cheat sheet
- `map` - transform the successful (right) value, if successful
- `flatMap` - run a transformation which produces a new validation, depending on the previous result
- `orSuccess` - return this value if successful, or the default value otherwise
- `fj.data.Validation#validation(fj.F<E,X>, fj.F<T,X>)` - catamorphism
- `toEither` - convert back to either

## Exercise

- A person has ID (int), Name (non-empty string), Age (positive int) and Gender (enum Gender{m,f,other})
- Read two people from the console
- If successful, check if both people above 21
- If successful, check if one person is male and other is female
- If successful, return a match object containing the two people's ids
- Make sure you have tests
    
    


