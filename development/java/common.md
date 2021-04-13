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

try-with-resources suppresses exception thrown by close method if an execution exception is thrown:

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

