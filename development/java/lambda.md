# Lambda

## java.util.function

`UnaryOperator<T>` is a `Function<T, T>`. `<Double/Int/Long>UnaryOperator` operates with primitives.

`ToDoubleFunction<T>`, `ToIntFunction<T>`, `ToLongFunction<T>` returns primitive out of one argument.  
`ToDoubleBiFunction<T, U>`, `ToIntBiFunction<T, U>`, `ToLongBiFunction<T, U>` returns primitive out of two arguments.

`Consumer<T>` consumes a value. `DoubleConsumer`, `IntConsumer`, `LongConsumer` consume primitive, thus does not extend `Consumer`.

`Function<T, R>` converts `T` to `R`. `BiFunction<T, U, R>` converts `T` and `U` to `R`.  `BinaryOperator<T>` is a `BiFunction<T,T,T>`. `DoubleBinaryOperator`, `IntBinaryOperator`, `LongBinaryOperator` operate with respective primitives thus does not extend `BinaryOperator`.

`DoubleFunction<R>`, `IntFunction<R>` and `LongFunction<R>` conver respective primitive to `R`. `DoubleToIntFunction`, `DoubleToLongFunction`, `IntToDoubleFunction`, `IntToLongFunction`, `LongToDoubleFunction`, `LongToIntFunction` operate with primitives.

## Definition

When boxed types are used as generics for functional interface then primitive types cannot be used in the definition:

```text
ToDoubleBiFunction<Double, Double> f = (double o, double o2) -> o + o2; // not allowed
ToDoubleBiFunction<Integer, Double> f2 = (Integer o, Double o2) -> o + o2; // ok
```



