# Types

## Interface Structure

Interfaces are implicitly abstract;

Abstract/default methods cannot be final.

When to interfaces defines a default method with the same signature then the class that extends both needs to provide an explicit implementation.

```text
interface A {
    default void m() {
        System.out.println("A");
    }
}

interface B {
    default void m() {
        System.out.println("B");
    }
}

class C implements A, B {
    public void m() {
        A.super.m();
    }
}
```

An interface that has one abstract method plus any method inherited from Object is still functional.

```text
@FunctionalInterface
interface A {
    void m();
    String toString();
}
```

All the fields must be initialized as they are implicitly final.

## Class Structure

Static declarations are not allowed in inner \(non-static\) classes.

Static and non-static methods cannot share the same name.

Members of the inner classes can be accessed in the following way:

```text
class A {
    int a = 0;
    
    class B {
        int b = 1;
        
        void m() {
            System.out.println(A.this.a);
            System.out.println(A.B.this.b);
        }
    }
}
```

Local classes can access only final variables:

```text
class A {
    void m() {
        int i = 1;
        // i = 2; // must be final
        
        class B {
            void m() {
                System.out.println(i);
            }
        }
    }
}
```

Static initializers can be skipped if containing class is not loaded at all.

## Enum Structure

Enums are implicitly static.

Constructors can be private only.

## Lambda

Any instance fields cannot be accessed from within the lambda, but only final local variables can be:

```text
static class Test {
    int i = 1;

    public static void main(String[] args) {
        Test t = new Test();
        t.i = 2;

        Supplier<Integer> s1 = () -> t.i;
        
        int i2 = 3;
        // i2 = 4; // local var must be final
        Supplier<Integer> s2 = () -> i2;
    }
}
```

Functional interfaces can operate with primitives:

```text
@FunctionalInterface
interface P {
    int i();
}

public static void main(String[] args) {
    P p = () -> 1;
}
```

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

Child class hides fields with the same name and static methods with the same signature:

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

While overriding methods return types must stay covariant:

```text
class A {
    public Number n(Number p) {
        return null;
    }
}

class B extends A {

    @Override
    public Long n(Number p) { // return type must be covariant and compatible
        return null;
    }
}
```

Either `this()` or `super()` can be used in a constructor at a time.

Class fields are accessed by the concrete reference type, not the underlying object type:

```text
static class A {
    int i = 1;
}

static class B extends A {
    int i = 2;
}

public static void main(String[] args) {
    A a = new B();
    System.out.println(a.i); // 1
}
```

Methods are accessed by the underlying object type, not the reference type: 

```text
static class A {
    int i = 1;
    
    public int getI() {
        return i;
    }
}

static class B extends A {
    int i = 2;

    public int getI() {
        return i;
    }
}

public static void main(String[] args) {
    A a = new B();
    System.out.println(a.getI()); // 2
}
```

## Overloading

When matching the method by parameters primitive types are promoted first, otherwise boxed:

```text
static void m(int i) {
    System.out.println("int");
}

// if this method does not exist then 'Float' printed out
static void m(double d) {
    System.out.println("double");
}

static void m(Float f) {
    System.out.println("Float");
}

public static void main(String[] args) {
    m(1.0f); // double
}
```

