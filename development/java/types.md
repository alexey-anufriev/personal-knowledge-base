# Types

## Interface Structure

Abstract/default methods cannot be final.

## Class Structure

Static declarations are not allowed in inner \(non-static\) classes.

## Instantiation

Inner classes instantiation

```text
static class A {
    int val = 1;

    class B {
        int getVal() {
            return val;
        }
    }

    static class C {}
}

public static void main(String[] args) {
    A a = new A();
    a.val = 5;

    A.B b1 = new A().new B();
    System.out.println(b1.getVal()); // 1

    A.B b2 = a.new B();
    System.out.println(b2.getVal()); // 5

    A.C c = new A.C();
}
```

