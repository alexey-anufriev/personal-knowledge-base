# Common

## Variables

`_` \(single underscore\) is not a valid variable name.  
`var a` is not allowed without value.

```text
// compilation fail, `str` is not initialized yet
String str = "123".substring(str.length());
```

## Operators Precedence

| Level | Operator | Description | Associativity |
| :--- | :--- | :--- | :--- |
| **16** | `[] . ()` | access array element access object member parentheses | left to right |
| **15** | `++ --` | unary post-increment unary post-decrement | not associative |
| **14** | `++ -- + - ! ~` | unary pre-increment unary pre-decrement unary plus unary minus unary logical NOT unary bitwise NOT | right to left |
| **13** | `() new` | cast object creation | right to left |
| **12** | `* / %` | multiplicative | left to right |
| **11** | `+ - +` | additive string concatenation | left to right |
| **10** | `<< >> >>>` | shift | left to right |
| **9** | `< <= > >= instanceof` | relational | not associative |
| **8** | `== !=` | equality | left to right |
| **7** | `&` | bitwise AND | left to right |
| **6** | `^` | bitwise XOR | left to right |
| **5** | `|` | bitwise OR | left to right |
| **4** | `&&` | logical AND | left to right |
| **3** | `||` | logical OR | left to right |
| **2** | `?:` | ternary | right to left |
| **1** |  `=   +=   -= *=   /=   %= &=   ^=   |= <<=  >>= >>>=` | assignment | right to left |

```text
int a = 1, b = ++a * 2; // 4

int c = 2 + 3 * 4; // 2 + 12

boolean d = 5 > 2 + 4; // 5 > 6

boolean e = 5 + 2 == 6; // 7 == 6

boolean f = false == true || true == true; // false || true

int g = 3;
boolean h = g > 4 | --g < 3; // | - eager operator, thus g = 2, h = true
```

## Flow Control

```text
// default statement in switch can be at any position

int a = 0;
switch (a) {
    case 1:
        System.out.println(1);
        break;

    default:
        System.out.println("d");
        break;

    case 2:
        System.out.println(2);
        break;
}
```

A `switch` statement supports the primitive types `byte`, `short`, `char`, and `int` and their associated wrapper classes `Character`, `Byte`, `Short`, and `Integer`. It also supports the `String` class and enumerated types. Finally, it permits `var` under some circumstances, such as if the type can resolve to one of the previous types.

## Advanced `break/continue`

```text
loop:
for (int i = 1; i < 10; i++) {
    System.out.println(i);
    break loop; // immediate exit from the labeled statement
}
System.out.println("done");

// 1
// done
```

```text
loop:
for (int i = 1; i < 3; i++) {
    for (int j = 1; j < 3; j++) {
        System.out.println("i-" + i);
        System.out.println("j-" + j);
        continue loop; // inner loop runs only one iteration every time
    }
}

// i-1
// j-1

// i-2
// j-1
```

## Casting

`null` is not possible to cast to primitive:

```text
List<Integer> ints = new ArrayList<>();
ints.add(null);

Integer I = ints.get(0);
int i = ints.get(0); // unboxing throws NullPointerException
```

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

## Call to Default Interface Method

```text
interface A {
    default void m() {
        // some logic
    }
}

interface B {
    default void m() {
        // some logic
    }
}

class C implements A, B {
    public void m() {
        A.super.m(); // call to a default method
    }
}
```

## Exceptions

`try-with-resources` suppresses exception thrown by `close` method if an execution exception is thrown:

```text
var closeable = new AutoCloseable() {

    @Override
    public void close() throws Exception {
        throw new Exception("close");
    }
};

try (closeable) {
    throw new Exception("exec");
}
catch (Exception e) {
    e.printStackTrace();
}

//java.lang.Exception: exec
// at Test.main(FlowControl.java:18)
// Suppressed: java.lang.Exception: close
//  at Test$1.close(FlowControl.java:13)
//  at Test.main(FlowControl.java:17)
```

Resource supplied for `try-with-resource` must be at least effectively final. They are closed in reverse order as they were supplied.

Catch block must be specified for exceptions in order from most specific exception to less specific:

```text
// not allowed

try {
}
catch (RuntimeException e) {
}
catch (NullPointerException e) {
}
```

Catch cannot contain checked exceptions that were not thrown by the wrapped code block.

When both, catch and finally blocks throw exceptions then the one from catch block is suppressed by the one from finally block:

```text
try {
    throw new NullPointerException();
}
catch (RuntimeException e) {
    throw new ArithmeticException();
}
finally {
    throw new IllegalArgumentException();
}

// Exception in thread "main" java.lang.ArithmeticException
```

## Generics

When using concrete type on the left side in the definition then it must be the same on the right side. But types can be different when using lower/upper bounds:

```text
// not allowed
List<Number> list = new ArrayList<Integer>();

// allowed
List<? extends Number> list2 = new ArrayList<Integer>();
```

Comparison of lower-bound and upper-bound restrictions:

```text
static class Creature {}
static class Animal extends Creature {}
static class Tiger extends Animal {}
static class Lion extends Animal {}

// lower bound

List<? super Animal> list = new ArrayList<>(); // can be also Object

list.add(new Tiger()); // ok, as list contains supertypes
list.add(new Lion()); // ok, as list contains supertypes

Creature creature = list.get(0); // error, cast required, can be also Object

// upper bound

List<? extends Animal> list = new ArrayList<>(); // can be any subtype

list.add(new Tiger()); // error, cannot add as it can be any subtype of the Animal
list.add(new Lion()); // error, cannot add as it can be any subtype of the Animal

Lion lion = list.get(0); // error, cast required, it can be any subtype of the Animal
```

## Classes

Member inner class cannot contain static methods.

Local class can access all local variables that are at least effectively final.

