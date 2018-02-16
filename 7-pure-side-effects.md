# Handling side effects purely

As we saw, we can 'lift' arbitrary code which throws exceptions to a **pure**
value by utilizing `Validation<E, A>`.

Now we want to go a step further, and threat code which performs side effects
as a pure value. Side effects incude:

- Reading / writing from a file;
- Reading / writing from stdin / stdout;
- Calling an external service;
- Calling a method on a class which relies on mutable fields;
- Etc;

The above things have in common that they are not referentially transparent - 
if I call that piece of code several times with the same input, the output
may be different.

## The trick

While a function **performing a side effect** is never pure, I can lift it to a pure function
by returning a value **describing** the side effect that is to be performed. Consider

```java
    static String readFromConsole()
    {
        return new Scanner(System.in).nextLine();
    }
```

I can lift this to a pure function, by utilizing laziness:

```java
    static F0<String> readFromConsolePure() 
    {
        return () -> readFromConsole();
    }
```

Now my function is pure. Everytime I call it, it returns the same thing - a **value** which describes
reading a line from the console.

And a value it is indeed. I can transform it via a function, and get a new value:

```java
    P1<String> val = lazy(readFromConsolePure);
    P1<Integer> len = val.map(str -> str.length())
```

*(A `P1` is a wrapper type around a `F0` which allows me to transform it via `bind` and `map`.
The corresponding wrapper types for `F1` and `F2` are `F1W` and `F2W`)*

No side effects are performed yet. I can continue these transformations as long as I need:

```java
    P1<Integer> len2 = lazy(readFromConsolePure).map(x -> x.len);
    P1<Integer> maxLen = len1.bind(x -> len2.map(y -> Math.max(x, y)));
```


At some point in the program I will eventually need to run the effectful computation and obtain its result
(and effect). We delay this as long as possible, and only restrict the evaluation of the side effect
to the edge of our program - e.g. the main() method, the web service controller, the @Test, etc.

I run the effectful computation by calling `f()` and forcing the value to evaluate:

```java
    //perform the side effects
    Integer max = maxLen.f();
    System.out.println("max length:" + max);
```  

The rest of our program is pure.


## Side effects with exceptions

Side-effecting functions, more often than not, throw exceptions. I.e. they are not of the form `F0<A>`,
but of the form `Try0`:

```java
public interface Try0<A, Z extends Exception> {

    A f() throws Z;

}
```

We already know how to deal with exceptions. We will lift such code to pure form, by using
`P1<Validation<Exception, A>>` instead of simply `P1<A>`:

```java
P1<Validation<Exception, A>> pure = Try.f(() -> someImpureMethod())
```
, where 
```java
    static A someImpureMethod() throws Exception
    {
        //here be dragons
    }
```

You can have a look at `org.example.chapter7.TryExample` for a more complex example.

## Exercise

Implement `org.example.chapter7.FileUtil`.
