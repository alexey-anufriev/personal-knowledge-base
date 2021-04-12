# Types

## Interface Structure

Interfaces are implicitly abstract;

Abstract/default methods cannot be final.

When to interfaces defines a default method with the same signature then the class that extends both needs to provide an explicit implementation. 

## Class Structure

Static declarations are not allowed in inner \(non-static\) classes.

Static and non-static methods cannot share the same name.

## Enum Structure

Enums are implicitly static.

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

## Inheritance

Child class hides fields with the same name and static methods with the same signature.

```text
static class A {
    int val = 1;

    static void m() {
        System.out.println("A");
    }
}

static class B extends A {
    int val = 3;

    static void m() {
        System.out.println("B");
    }
}

public static void main(String[] args) {
    System.out.println(new B().val); // 3
    B.m(); // B
}
```

