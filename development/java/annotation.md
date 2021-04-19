# Annotation

## Common

An annotation element type must be a primitive type, a `String`, a `Class`, an `enum`, another annotation, or an array of any of these types. `null` is not allowed as a default. Annotation elements must be public, implicitly or explicitly.

`@SuppressWarnings` annotation indicates that the named compiler warnings should be suppressed in the annotated element.

Annotation can be inherited by subtypes, also they can be visible in the runtime:

```text
@Retention(RetentionPolicy.RUNTIME)
@Inherited // inherited by subtypes
@interface An {}

@An static class T1 {}
static class T2 extends T1 {}

public static void main(String[] args) {
    System.out.println(T2.class.getAnnotation(An.class)); // prints @Test$An()
}
```

Multiple annotations can be applied at once:

```text
@Repeatable(Collection.class)
@interface Item {
    int id();
}
    
@interface Collection {
    Item[] value();
}

@Collection({ @Item(id = 1), @Item(id = 2) })
static class T {}
```

`@SafeVarargs` is not allowed on non-final instance methods.

