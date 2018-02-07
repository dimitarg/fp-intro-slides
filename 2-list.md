# Functional data structures - List

## Exercise recap - chapter 1

### Parametricity

- The functions `id`, `const`, `compose` `curry`, `uncurry` from last time have only one possible implementation
which compiles
    - *(that is, if you use a sane subset of Java)*
- This is because of generics:
```java
    <A> Function<A, A> identity();
```
- If we were not generic in the types, this would not work:
```java
    Function<Integer, Integer> foo()
    {
        return x -> 42;
    }
    
    Function<Integer, Integer> bar()
    {
        return x -> x * 2;
    }
    // millions more ...
```
- `<A> Function<A, A> foo()` means `for all A`
- By letting ourselves know nothing about `A`, we constrain the number of possible implementations
- This is called **parametricity**. It gives us the implementation **for free**.
- We will do this a lot


## Data structures

We need data structures in order to write code. The JDK std data structures will not provide 
the modularity and composition we need. We will need new ones. 

## Functional data is immutable

- State-mutating methods are not functions
    - `java.util.Collection#add`
    - `java.util.Collection#remove`
- State-dependent methods are not functions
    - `java.util.Collection#iterator`
    - `java.util.List#get`
    
- Functions cannot rely on, or mutate state
    - Otherwise they will break the definition of function
    
Therefore, functional data structures are immutable by definition.

## List definition

A list is either

- The empty list, or
- An element followed by another list

Notice the definition of `List` is recursive; `List` is defined in terms of `List`.

> A type which can be one of a predefined set of cases is called an Algebraic Data Type (ADT) 

> An ADT which is recursive is called a Generalized ADT (GADT)

### List definition in Scala

Roughly,

```scala
  sealed trait List[A]
  case object Nil extends List[Nothing]
  case class Cons[A](head: A, tail: List[A]) extends List[A]
  
  // ...
  
  // a function matching on a list
  def size[A](xs: List[A]) : Int = xs match {
    case Nil => 0
    case Cons(y, ys) => 1 + size(xs)
  }
```

### List definition in Java

Java has no support / syntax sugar for ADTs and GADTs.

```java

public abstract class ConsList<A>
{

    public static <A> ConsList<A> empty()
    {
        return new Nil<>();
    }

    public static <A> ConsList<A> cons(A head, ConsList<A> tail)
    {
        return new Cons<>(head, tail);
    }

    public abstract boolean isEmpty();

    public abstract A head();

    public abstract ConsList<A> tail();

    private static final class Cons<A> extends ConsList<A>
    {
        public final A head;
        public final ConsList<A> tail;

        public Cons(A head, ConsList<A> tail)
        {
            this.head = head;
            this.tail = tail;
        }

        @Override public boolean isEmpty()
        {
            return false;
        }

        @Override public A head()
        {
            return head;
        }

        @Override public ConsList<A> tail()
        {
            return tail;
        }
    }


    private static final class Nil<A> extends ConsList<A>
    {

        public Nil()
        {
        }

        @Override public boolean isEmpty()
        {
            return true;
        }

        @Override public A head()
        {
            throw error("head() on Nil");
        }

        @Override public ConsList<A> tail()
        {
            throw error("tail() on Nil");
        }
    }
    
    // ...
    
    // a function matching on a List
    public static  <A> int size(ConsList<A> xs) 
    {
        if (xs.isEmpty())    
        {
            return 0;
        }
        else 
        {
            return 1 + size(xs.tail());
        }
    }
}    
```


### A few notes on the definition

#### List instances are values

List instances are (immutable) values, just like `42` or `"fooblebar"`. Once constructed,
a value can never change.

#### Changing lists returns new values

- Adding an element to the front of a list
```java
    ConsList<String> xs = cons("a", cons("b"), cons("c"), empty()); // ["a", "b", "c"]
    ConsList<String> ys = cons("foo", xs); // ["foo", "a", "b", "c"]
```

- Dropping the first element of a list (if it exists)
```java
    if (!ys.isEmpty())
    {
         return ys.tail();   
    }
    else
    {
        return ys;
    }
    
```

#### Data is shared, not copied

```java
    ConsList<String> xs = ...;
    // xs is shared, no copying / allocation
    ConsList<String> ys = cons("foo", xs);
```

#### Asymptotic complexity

- `cons` - `O(1)`
- `head` - `O(1)`
- `tail` - `O(1)`
- length - `O(n)`
- appending to end of list - `O(n)`
- accessing last element - `O(n)`
- Reversing a list - `O(n)`
- Concatenating two lists - `O(n1)`, where `n1` is the size of the first list
- Random access = `O(m)`, where `m` is the index (not done)

Not all problems can be solved efficiently with a list.

## Exercise

Implement the following functions:

- `length`
- `append`
    - appends an element at the end of the list
- `reverse`
- `takeWhile`
    - takes elements in the list, until a predicate is true
    (predicate - Function<A, Boolean>), and returns a list
    containing those elements
- `dropWhile`
    - discards elements from the list while the predicate is true,
    and return a list of the rest of the elements
- `filter`
    - returns the sublist of this list, for which the given predicate is true
- `map`
    - (List<A>, Function<A,B>) -> List<B>
    - i.e. apply a function to each element of the list, returning a new list









