# 9️⃣ Interface Declaration

## 🔹 Definition

An interface is a contract that specifies a set of abstract methods that a class must implement.

It defines what a class must do, but not how it does it.

---

## 🔹 Important Points

- Interface is a reference type.
- Object creation is not allowed.
- Constructors are not allowed.
- A class must implement all abstract methods.
- Supports multiple inheritance through interfaces.

---

## 🔹 Default Behavior (Internal Rules)

- All variables are: `public static final`
- All methods are: `public abstract` (except default, static, private)

Example:

```java
interface Test {

    int x = 10;

    void m1();
}
```

Compiler internally treats it as:

```java
public abstract interface Test {

    public static final int x = 10;

    public abstract void m1();
}
```

---

## 🔹 Implementation Example

```java
interface Printable {
    void print();
}

class Demo implements Printable {

    public void print() {
        System.out.println("Printing...");
    }

    public static void main(String[] args) {
        Demo d = new Demo();
        d.print();
    }
}
```

---

## 🔹 Interface Reference Variable

An interface reference variable can refer to any object of the implementing class.

```java
Printable p = new Demo();
p.print();
```

This supports runtime polymorphism.

---

## 🔹 Interface Extending Another Interface

Interfaces can extend multiple interfaces.

```java
interface A {
    void m1();
}

interface B {
    void m2();
}

interface C extends A, B {
    void m3();
}
```

---

## 🔹 Default Methods (Java 8 Feature)

From Java 8, interfaces can have default methods with body.

```java
interface Vehicle {

    void start();

    default void fuelType() {
        System.out.println("Petrol");
    }
}
```

Purpose:
- To add new methods without breaking existing implementations.

---

## 🔹 Static Methods in Interface (Java 8 Feature)

Interfaces can contain static methods.

```java
interface Utility {

    static void show() {
        System.out.println("Static Method");
    }
}
```

Call using:

```java
Utility.show();
```

Static methods are not inherited.

---

## 🔹 Private Methods in Interface (Java 9 Feature)

Interfaces can contain private methods.

Used to reduce code duplication inside default methods.

```java
interface Test {

    default void m1() {
        common();
    }

    private void common() {
        System.out.println("Common Code");
    }
}
```

---

## 🔹 Marker Interface

An interface with no methods is called Marker Interface.

Purpose:
- To provide runtime information.

Examples:
- Serializable
- Cloneable
- RandomAccess

---

## 🔹 Functional Interface (Java 8)

An interface with exactly one abstract method is called Functional Interface.

Also known as SAM (Single Abstract Method) Interface.

```java
@FunctionalInterface
interface Interf {
    void m1();
}
```

Used in:
- Lambda expressions
- Stream API

---

## 🔹 Interface vs Abstract Class

| Feature | Interface | Abstract Class |
|----------|-----------|----------------|
| Methods | Abstract + default + static | Abstract + concrete |
| Variables | public static final only | Any type |
| Constructor | Not allowed | Allowed |
| Multiple Inheritance | Supported | Not supported |
| Object Creation | Not allowed | Not allowed |

---

## 🔹 Important Interview Points

- While implementing, method must be declared `public`.
- We cannot reduce visibility.
- Interface supports loose coupling.
- Multiple interfaces can be implemented by a single class.
- Before Java 8, interface provided 100% abstraction.
