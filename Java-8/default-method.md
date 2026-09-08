# Default Methods & Static Methods in Interface (Java 8)

Before Java 8:
Interface could contain only abstract methods.

Problem:
If we add new method in interface → all implementing classes break.

This breaks backward compatibility.

So Java introduced:
Default Methods

---

## Default Method

Method inside interface with body.

Declared using `default` keyword.

### Example

```java
interface Vehicle {

    default void start() {
        System.out.println("Vehicle starting...");
    }
}
```

---

### Implementation

```java
class Car implements Vehicle {
}

public class Test {
    public static void main(String[] args) {
        Car c = new Car();
        c.start();
    }
}
```

No need to override ✔

---

## Why Default Method?

To add new functionality without breaking old implementation classes.

Backward compatibility maintained.

---

## Overriding Default Method

```java
class Car implements Vehicle {
    public void start() {
        System.out.println("Car starting...");
    }
}
```

Child class can override default method.

---

## Multiple Inheritance Problem (Diamond Problem)

```java
interface Left {
    default void m1() {
        System.out.println("Left");
    }
}

interface Right {
    default void m1() {
        System.out.println("Right");
    }
}
```

```java
class Test implements Left, Right {

    public void m1() {
        Left.super.m1();
    }
}
```

We must override method to resolve conflict.

---

## Static Methods in Interface

Java 8 allows static methods inside interface.

### Example

```java
interface Interf {
    static void m1() {
        System.out.println("Interface static method");
    }
}
```

Call using:

```java
Interf.m1();
```

---

## Important Rules

| Default Method | Static Method |
|------|------|
| inherited | not inherited |
| override allowed | override not allowed |
| called by object | called by interface name |

---

## Key Interview Point

Default methods support multiple inheritance in Java safely.
