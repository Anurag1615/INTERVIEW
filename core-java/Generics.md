# Generics in Java

---

## 🔹 What Are Generics?

Generics allow classes, interfaces, and methods to operate on **types specified as parameters**, providing **compile-time type safety** without casting.

Introduced in **Java 5**.

### Without Generics (Before Java 5)

```java
List list = new ArrayList();
list.add("hello");
list.add(10); // no compile-time check — mixed types allowed

String s = (String) list.get(1); // ❌ ClassCastException at runtime!
```

### With Generics

```java
List<String> list = new ArrayList<>();
list.add("hello");
// list.add(10); // ❌ Compile-time error — caught early!

String s = list.get(0); // ✔ No cast needed
```

---

## 🔹 Generic Class

```java
class Box<T> {
    private T value;

    void set(T value) { this.value = value; }
    T get() { return value; }
}

Box<Integer> intBox = new Box<>();
intBox.set(10);

Box<String> strBox = new Box<>();
strBox.set("hello");
```

`T` is a **type parameter** — a placeholder for whatever type is supplied at usage time.

---

## 🔹 Generic Method

```java
class Util {
    static <T> void printArray(T[] array) {
        for (T item : array) {
            System.out.println(item);
        }
    }
}

Integer[] intArr = {1, 2, 3};
String[] strArr = {"a", "b", "c"};

Util.printArray(intArr);
Util.printArray(strArr);
```

---

## 🔹 Bounded Type Parameters

Restrict the type parameter to a specific type or its subtypes.

```java
class Calculator<T extends Number> {
    T num;
    Calculator(T num) { this.num = num; }

    double square() {
        return num.doubleValue() * num.doubleValue(); // Number methods available
    }
}

Calculator<Integer> c1 = new Calculator<>(5);   // ✔ valid
// Calculator<String> c2 = new Calculator<>("x"); // ❌ compile error, String is not a Number
```

---

## 🔹 Wildcards (`?`)

Used when the exact type is unknown, typically in method parameters.

### Unbounded Wildcard — `<?>`

```java
void printList(List<?> list) {
    for (Object o : list) {
        System.out.println(o);
    }
}
```

### Upper Bounded Wildcard — `<? extends T>`

Accepts `T` or any subtype. Used when you only need to **read** from the structure (producer).

```java
void printNumbers(List<? extends Number> list) {
    for (Number n : list) {
        System.out.println(n);
    }
}

printNumbers(List.of(1, 2, 3));       // List<Integer> — OK
printNumbers(List.of(1.5, 2.5));      // List<Double> — OK
```

### Lower Bounded Wildcard — `<? super T>`

Accepts `T` or any supertype. Used when you need to **write/add** into the structure (consumer).

```java
void addNumbers(List<? super Integer> list) {
    list.add(10);
    list.add(20);
}

List<Number> numberList = new ArrayList<>();
addNumbers(numberList); // ✔ Number is a supertype of Integer
```

### PECS Principle — "Producer Extends, Consumer Super"

- Use `extends` when the generic structure **produces** (you read from it).
- Use `super` when the generic structure **consumes** (you write into it).

---

## 🔹 Type Erasure

Java generics are implemented using **Type Erasure** — generic type information exists **only at compile time**; it is removed ("erased") at runtime for backward compatibility with pre-Java-5 code.

```java
List<String> stringList = new ArrayList<>();
List<Integer> intList = new ArrayList<>();

System.out.println(stringList.getClass() == intList.getClass()); // true! Both are just List.class at runtime
```

### Implications of Type Erasure

- You **cannot** create a generic array: `T[] arr = new T[10];` ❌
- You **cannot** use `instanceof` with a parameterized type: `if (obj instanceof List<String>)` ❌ (only `instanceof List` is allowed)
- You **cannot** overload methods that differ only in generic type: `void m(List<String> l)` and `void m(List<Integer> l)` ❌ (same erasure `List`)
- Static context cannot use class-level type parameters (`static T value;` ❌)

---

## 🔹 Comparison Table

| Wildcard | Meaning | Use Case |
|----------|---------|----------|
| `<?>` | Any type | When type doesn't matter, only reading generically |
| `<? extends T>` | `T` or subtype | Producer — reading data |
| `<? super T>` | `T` or supertype | Consumer — writing data |

---

## 🔥 Interview Questions

1. Why were Generics introduced in Java 5?
2. What is Type Erasure and what limitations does it impose?
3. What is the difference between `<? extends T>` and `<? super T>`? Explain PECS.
4. Why can't you create a generic array in Java?
5. Why does `List<String>` and `List<Integer>` have the same `.class` at runtime?

---

## 🎯 Summary

✔ Generics provide compile-time type safety, eliminating manual casting  
✔ Bounded types restrict allowed type parameters  
✔ Wildcards (`?`, `? extends`, `? super`) provide flexibility for method parameters  
✔ Type Erasure removes generic info at runtime — causes several restrictions (no generic arrays, no runtime type checks)
