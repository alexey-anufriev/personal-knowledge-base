# Streams

## Intermediate Operators

`flatMap` converts element into a `Stream`.

## Terminal Operators

Generic stream requires comparator to find min/max value but specific stream does not:

```text
IntStream i = IntStream.of(1, 2, 3);
System.out.println(i.min().getAsInt());

Stream s = Stream.of(1, 2, 3);
System.out.println(s.min(Comparator.comparingInt(o -> (Integer) o)).get());
```

Spefic numeric stream e.g. `DoubleStream`, `LongStream` or `IntStream` may return concrete values for `sum` \(0.0 if no elements\) and `count` \(0 if no elements\) operations, and optional values for `min`, `max`, `average` operations.

`accumulator` while reducing is used to combine identity with the element, and `combiner` is used for parallel reduction:

```text
String reduce = Stream.of(1, 2, 3, 4)
        .reduce("-", (i1, i2) -> i1+i2, (s, s2) -> s+ ":" + s2);
System.out.println(reduce); // -1234

String reduce = Stream.of(1, 2, 3, 4).parallel() // IMPORTANT
        .reduce("-", (i1, i2) -> i1+i2, (s, s2) -> s+ ":" + s2);
System.out.println(reduce); // -1:-2:-3:-4
```

