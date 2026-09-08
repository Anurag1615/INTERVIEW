# Functional Interface

Interface which contains exactly ONE abstract method.

Also called:
SAM Interface (Single Abstract Method)

---

## Example

```java
interface Interf {
    void m1();
}
```

Valid Functional Interface ✔

---

## Invalid Example

```java
interface Interf {
    void m1();
    void m2();
}
```

Not Functional ❌

---

## @FunctionalInterface Annotation

Used to restrict interface to only one abstract method.

If we add second abstract method → Compile time error.

```java
@FunctionalInterface
interface Interf {
    void m1();
}
```

---

## Important Rule

Functional interface can contain:

| Member Type | Allowed |
|------|------|
| Abstract Method | Only one |
| Default Method | Any number |
| Static Method | Any number |

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

Valid ✔

---



# Predefined Functional Interfaces (Java 8)

Java 8 provides some ready-made functional interfaces inside `java.util.function` package.

Most Important:
- Predicate
- Function
- Consumer
- Supplier

---

## 1. Predicate

Represents boolean valued function.

### Method
`boolean test(T t)`

### Example

```java
import java.util.function.Predicate;

Predicate<Integer> p = i -> i % 2 == 0;

System.out.println(p.test(10)); // true
System.out.println(p.test(7));  // false
```

### Joining Predicates

| Method | Meaning |
|------|------|
| and | both true |
| or | any true |
| negate | reverse |

```java
Predicate<Integer> greater10 = i -> i > 10;
Predicate<Integer> even = i -> i % 2 == 0;

System.out.println(greater10.and(even).test(20)); // true
System.out.println(greater10.or(even).test(8));   // true
System.out.println(even.negate().test(8));        // false
```

---

## 2. Function

Takes input and returns output.

### Method
`R apply(T t)`

```java
import java.util.function.Function;

Function<Integer,Integer> f = i -> i * i;

System.out.println(f.apply(5)); // 25
```

### Chaining

```java
Function<Integer,Integer> f1 = i -> i + 2;
Function<Integer,Integer> f2 = i -> i * i;

System.out.println(f1.andThen(f2).apply(3)); // 25
System.out.println(f1.compose(f2).apply(3)); // 11
```

---

## 3. Consumer

Takes input but returns nothing.

### Method
`void accept(T t)`

```java
import java.util.function.Consumer;

Consumer<String> c = s -> System.out.println(s);
c.accept("Hello");
```

---

## 4. Supplier

No input but returns value.

### Method
`T get()`

```java
import java.util.function.Supplier;

Supplier<Integer> s = () -> (int)(Math.random() * 100);
System.out.println(s.get());
```

---

## Memory Trick

| Interface | Input | Output |
|------|------|------|
| Predicate | Yes | Boolean |
| Function | Yes | Yes |
| Consumer | Yes | No |
| Supplier | No | Yes |


## Why Lambda requires Functional Interface?

Lambda provides implementation for only one method.

So multiple abstract methods → confusion for compiler.
