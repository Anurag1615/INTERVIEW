# JAVA 8 — Lambda Expressions (Durga Sir Notes)

---

## What is the need of Java 8 ?

Before Java 8, Java was purely Object Oriented Programming language.

For even a very small task we had to:

- Create class
- Create object
- Override method

Even for single method logic 😑

---

### Example: Runnable (Before Java 8)

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Child Thread");
    }
}

public class Test {
    public static void main(String[] args) {
        MyRunnable r = new MyRunnable();
        Thread t = new Thread(r);
        t.start();
    }
}
```

👉 **Problem:**  
Too much boilerplate code for very small logic.

So Java designers thought:

> "Why not treat behaviour as a value?"

Then Java introduced **Functional Programming support**

Java 8 introduced:

- Lambda Expressions
- Functional Interfaces
- Streams
- Method References

---

## Lambda Expression

### Definition
Lambda Expression is an anonymous function.

It does not have:

- Name
- Return type declaration
- Modifiers

---

### Syntax
```
(parameters) -> { body }
```

---

### Examples

#### Example 1
```java
() -> System.out.println("Hello");
```

#### Example 2
```java
(a,b) -> System.out.println(a+b);
```

#### Example 3
```java
n -> n*n;
```

---

### Important Rule

If body contains only one statement:

- Curly braces `{}` optional
- `return` keyword optional

---

## Functional Interface

### Definition
An interface which contains **exactly one abstract method** is called Functional Interface.

Also known as:

> SAM Interface (Single Abstract Method)

---

### Valid Example

```java
interface Interf {
    public void m1();
}
```

✔ Valid Functional Interface

---

### Invalid Example

```java
interface Interf {
    void m1();
    void m2();
}
```

❌ Not Functional Interface

---

## @FunctionalInterface Annotation

Used for compile-time checking.

If more than one abstract method is present → Compile time error.

---

### Important Interview Point

Functional interface can contain:

| Type | Allowed |
|------|------|
| Abstract method | Only ONE |
| Default methods | Any number |
| Static methods | Any number |

---

### Example

```java
@FunctionalInterface
interface Interf {
    void m1();

    default void m2() {}
    static void m3() {}
}
```

✔ Valid Functional Interface

---

## Why Lambda works only with Functional Interface ?

Because Lambda provides implementation for only one method.  
Otherwise compiler confusion ho jayega.

---

## Functional Interface Example using Lambda

```java
interface Interf {
    public void m1();
}

public class Test {
    public static void main(String[] args) {

        Interf i = () -> System.out.println("Hello");

        i.m1();
    }
}
```

---

## Runnable using Lambda

### Before Java 8

```java
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Child Thread");
    }
};
```

### After Java 8

```java
Runnable r = () -> System.out.println("Child Thread");

Thread t = new Thread(r);
t.start();
```

👉 Huge code reduction

---

## Comparator using Lambda

### Before Java 8

```java
Comparator<Integer> c = new Comparator<Integer>() {
    public int compare(Integer i1, Integer i2) {
        return i1 - i2;
    }
};
```

### After Java 8

```java
Comparator<Integer> c = (i1,i2) -> i1 - i2;
```

---

## Conclusion

Lambda Expression is used to:

- Reduce length of code
- Improve readability
- Enable Functional Programming in Java
