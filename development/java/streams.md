# Streams

## Terminal Operators

Generic stream requires comparator to find min/max value but specific stream does not:

```text
IntStream i = IntStream.of(1, 2, 3);
System.out.println(i.min().getAsInt());

Stream s = Stream.of(1, 2, 3);
System.out.println(s.min(Comparator.comparingInt(o -> (Integer) o)).get());
```

