# Streams

## General

Stream cannot be reused.

Sets are unordered but lists are ordered:

```text
System.out.println(Set.of().stream().spliterator().hasCharacteristics(ORDERED));
// false

System.out.println(List.of().stream().spliterator().hasCharacteristics(ORDERED));
// true
```

## Intermediate Operators

`flatMap` converts element into a `Stream`. `flatMapToInt`, `flatMapToDouble`, `flatMapToLong` requires conversion into respective stream \(e.g. `IntStream`\) but not a generic stream.

`sorted` operator may cause the execution to never complete if applied for infinite stream:

```text
Optional<Integer> first = Stream.generate(() -> 1).sorted().findFirst();
System.out.println(first.get()); // never reached
```

`IntStream.boxed()` converts a stream into `Stream<Integer>`. `Stream.mapToInt()` converts the stream back to `IntStream`.

## Terminal Operators

Generic stream requires comparator to find min/max value but specific stream does not:

```text
IntStream i = IntStream.of(1, 2, 3);
System.out.println(i.min().getAsInt());

Stream s = Stream.of(1, 2, 3);
System.out.println(s.min(Comparator.comparingInt(o -> (Integer) o)).get());
```

Spefic numeric stream e.g. `DoubleStream`, `LongStream` or `IntStream` may return concrete values for `sum` \(0.0 if no elements\) and `count` \(0 if no elements\) operations, and optional values for `min`, `max`, `average` operations. Also these streams return statistics with concrete values \(not special treatment for empty streams\):

```text
IntStream i = IntStream.empty();
System.out.println(i.min().getAsInt()); // exception, no value present
System.out.println(i.summaryStatistics().getMax()); // -2147483648
```

Numeric streams can map values to anothe primitives or objects:

```text
LongStream.of(9).mapToDouble(x -> x);
LongStream.of(9).mapToInt(x -> (int) x);
LongStream.of(9).mapToObj(x -> x);

IntStream.of(9).mapToDouble(x -> x);
IntStream.of(9).mapToLong(x -> x);
IntStream.of(9).mapToObj(x -> x);

DoubleStream.of(9).mapToLong(x -> (long) x);
DoubleStream.of(9).mapToInt(x -> (int) x);
DoubleStream.of(9).mapToObj(x -> x);
```

`accumulator` while reducing is used to combine identity with the element, and `combiner` is used for parallel reduction:

```text
String reduce = Stream.of(1, 2, 3, 4)
        .reduce("-", (i1, i2) -> i1+i2, (s, s2) -> s+ ":" + s2);
System.out.println(reduce); // -1234

String reduce = Stream.of(1, 2, 3, 4).parallel() // IMPORTANT
        .reduce("-", (i1, i2) -> i1+i2, (s, s2) -> s+ ":" + s2);
System.out.println(reduce); // -1:-2:-3:-4
```

To execute a parallel reduction with the `collect()` method, the stream or collector must be unordered, the collector must be concurrent, and the stream must be parallel.

Collecting into map can be done via `groupingBy` or `partitioningBy`:

```text
Map<Boolean, List<Integer>> map = Stream.of(1, 2, 3, 4, 5)
        .collect(Collectors.partitioningBy(num -> num > 3));
System.out.println(map); // {false=[1, 2, 3], true=[4, 5]}

Map<Boolean, List<Integer>> map2 = Stream.of(1, 2, 3, 4, 5)
        .collect(Collectors.groupingBy(num -> num > 3));
System.out.println(map2); // {false=[1, 2, 3], true=[4, 5]}
```

`for-each` can be ordered that forces parallel execution to be sequential:

```text
List.of("a", "b", "c", "d", "e").stream().parallel().forEachOrdered(System.out::print);
// abcde

List.of("a", "b", "c", "d", "e").stream().parallel().forEach(System.out::print);
// ceabd

List.of("a", "b", "c", "d", "e").stream().forEachOrdered(System.out::print);
// abcde

List.of("a", "b", "c", "d", "e").stream().forEach(System.out::print);
// abcde
```



