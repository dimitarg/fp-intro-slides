# Option

## Recap - chapter 3

- `foldLeft` encodes an imperative loop over a list
- `foldRight` encodes the list structure. `xs.foldRight(f, z)` simply replaces `:` with `f`
and `Nil` with `z`
- every list operation can be expressed as a `foldRight`
- it is not always a good idea to do so
- `foldRightC` is not equivalent to `foldRight`, in general

## Infrastructure

We now add the following library to our classpath:

```groovy
compile group: 'org.functionaljava', name: 'functionaljava', version: '4.7'
```

- `Function` becomes `fj.F`
- `BiFunction` becomes `fj.F2`
- `ConsList` becomes `fj.data.List`

## Nullness done wrong

```java
Fooble x = getTheFooble();
if (x == null)
{
    // ...       
}
else
{
    // ...
}
```

### Problems

- We have to remember to check for null
    - This doesn't scale
- Not clear when null is valid return, and when not

```java
Fooble fooble = getFooble(42); //get fooble by id, can be null
AppConfig config = AppConfig.instance(); // get the app config, always non-null
```
- We have to keep the whole codebase in our head

### The solution

- We would instead want the **type system** to keep track of nullness
- A `Fooble` and a *nullable* `Fooble` become different types
    - We don't have to remember to check for null - compiler will tell us
    - It becomes clear when a method can return null, without looking at its impl
    
```java
import fj.data.Option;
//..
Option<Fooble> fooble = getFooble(42);

```
    
## Undefinedness done wrong

Express the result of a computation that can be undefined for a given input.

```java
static Integer mean(List<Integer> xs)
{
    if (xs.isEmpty())
    {
        return null; // lame
    }
}
```

```java
static Integer mean(List<Integer> xs)
{
    if (xs.isEmpty())
    {
        throw new ArithmeticException();
        // super lame for many reasons
    }
}
```

```java
static Integer mean(List<Integer> xs)
{
    if (xs.isEmpty())
    {
        return -1; // or 0 ? ... not sure
    }
}
```

- All these solutions are non-satisfactory

- It may also not be possible to come up with a default value:
```java
enum Ordering{LT, EQ, GT}
static A max(List<A> xs, F2<A, A, Ordering> compare)
{
    if (xs.isEmpty())
    {
       //oh noez! how do I get a default A     
    }
}
```

### The solution
```java
import fj.data.Option;
```

#### Aside

We can go even further: we can constrain the **input** of the `max` and `mean` functions to 
not allow for an empty list at all. Then the result would **have** to be defined.

```java
static A max(fj.data.NonEmptyList<A> a, fj.Ord<A> ord);
```


## Option crash course

- Construct an optional value
```java
Option<String> x = some("shrubbery");
```

```java
Option<Integer> y = none();
```

- Check if a value is defined
```java
if (x.isSome())
{
    // ...        
}

if (y.isNone())
{
    // ...        
}
```


- Retrieve the value, if defined

```java
if (x.isSome())
{
    String foo = x.some(); // non-total, throws
}
```

- Transform a value, if it is defined
```java
Option<Integer> length = x.map(xx -> xx.length()); 
```

- Transform a value, where the result of the transformation is also optional:

```java
Option<Character> firstChar = x.bind(xx -> xx.length() > 0 ? some(xx.charAt(0)) : none());
```

- Transform an optional value to a regular value, given a default value
```java
String str = x.orSome("alabala");
```

- Do the same thing, lazily
```java
String str2 = x.orSome(() -> {
            System.out.println("will not be printed");
            return "alabala";
        });
```



