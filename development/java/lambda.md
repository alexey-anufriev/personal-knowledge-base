# Lambda

## java.util.function

`UnaryOperator<T>` is a `Function<T, T>`. `<Double/Int/Long>UnaryOperator` operates with primitives.

`ToDoubleFunction<T>`, `ToIntFunction<T>`, `ToLongFunction<T>` returns primitive out of one argument.  
`ToDoubleBiFunction<T, U>`, `ToIntBiFunction<T, U>`, `ToLongBiFunction<T, U>` returns primitive out of two arguments.

## Definition

When boxed types are used as generics for functional interface then primitive types cannot be used in the definition:

```text
ToDoubleBiFunction<Double, Double> f = (double o, double o2) -> o + o2; // not allowed
ToDoubleBiFunction<Integer, Double> f2 = (Integer o, Double o2) -> o + o2; // ok
```



