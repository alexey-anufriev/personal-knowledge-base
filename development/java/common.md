# Common

## Dynamic Class Casting

Casting is possible not only using explicit types but also using types stored in variables:

```text
Class<Integer> intType = Integer.class;
Object objVal = 1;
Integer intVal = intType.cast(objVal);
```

## Dynamic Array Operations

`Array.newInstance(elementType, length)` to generate a new array, it returns `Object`.

`Array.getLength(sourceArray)` to get the length of the array \(if represented with an `Object`\).

## Is Object Array?

`object.getClass().isArray()` tells if the object is an array.

## Labeled `break`

The `break` statement terminates the labeled statement; it does not transfer the flow of control to the label. Control flow is transferred to the statement immediately following the labeled \(terminated\) statement.

```text
String val = "";

loop:
do {
    val += "x";
    break loop;
} while (true);

System.out.println(val); // x
```

