# Wrapper Classes, Autoboxing & Unboxing

---

## 🔹 What Are Wrapper Classes?

Java is not a "pure" object-oriented language because it has 8 **primitive types** (`int`, `char`, `boolean`, etc.), which are not objects.

Wrapper classes provide an **object representation** for each primitive type, so primitives can be used where objects are required (e.g., in Collections, Generics).

| Primitive | Wrapper Class |
|-----------|----------------|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

---

## 🔹 Why Wrapper Classes Are Needed

1. **Collections require objects** — `List<int>` is invalid; `List<Integer>` works.
2. Provide **utility methods** — `Integer.parseInt()`, `Integer.MAX_VALUE`, `Character.isDigit()`.
3. Allow representing `null` for "no value" (impossible with primitives).

```java
List<Integer> list = new ArrayList<>(); // primitives can't be used directly
list.add(10); // autoboxed to Integer.valueOf(10)
```

---

## 🔹 Autoboxing & Unboxing

- **Autoboxing**: automatic conversion of primitive → wrapper object.
- **Unboxing**: automatic conversion of wrapper object → primitive.

```java
int a = 10;
Integer obj = a;      // Autoboxing: int → Integer  (compiler inserts Integer.valueOf(a))

Integer obj2 = 20;
int b = obj2;          // Unboxing: Integer → int    (compiler inserts obj2.intValue())
```

Introduced in **Java 5** to reduce boilerplate manual boxing/unboxing code.

---

## 🔹 Integer Caching (-128 to 127) ⭐ (Very Common Interview Trap)

```java
Integer i1 = 100;
Integer i2 = 100;
System.out.println(i1 == i2); // true → same cached object

Integer i3 = 200;
Integer i4 = 200;
System.out.println(i3 == i4); // false → different objects, outside cache range
```

### Why?

`Integer.valueOf()` internally maintains a cache for values from **-128 to 127** (`IntegerCache` class), since these are the most commonly used values. Values outside this range always create a new `Integer` object.

```java
// Inside java.lang.Integer (simplified)
public static Integer valueOf(int i) {
    if (i >= -128 && i <= 127) {
        return IntegerCache.cache[i + 128]; // reused cached object
    }
    return new Integer(i); // new object
}
```

This caching also applies to `Byte`, `Short`, `Long`, `Character` (0–127), and `Boolean`.

👉 **Always use `.equals()` to compare wrapper objects**, never `==` (except when you specifically want reference comparison).

---

## 🔹 NullPointerException Trap with Unboxing

```java
Integer obj = null;
int x = obj; // ❌ NullPointerException at unboxing (obj.intValue() on null)
```

Common in real code when a `Map.get()` returns `null` and gets auto-unboxed:

```java
Map<String, Integer> map = new HashMap<>();
int count = map.get("missingKey"); // ❌ NPE if key doesn't exist (get returns null)
```

---

## 🔹 Performance Consideration

Autoboxing/unboxing inside loops can hurt performance due to repeated object creation:

```java
Integer sum = 0;
for (int i = 0; i < 1_000_000; i++) {
    sum += i; // unboxes sum, adds, re-boxes into a NEW Integer each iteration!
}
```

Prefer primitive `int sum = 0;` for such loops instead of `Integer`.

---

## 🔥 Interview Questions

1. Why does Java need wrapper classes if it has primitives?
2. What is the Integer Cache and why does `Integer i1 = 100; Integer i2 = 100;` give `i1 == i2` as `true`, but `200` gives `false`?
3. What causes `NullPointerException` during unboxing?
4. Why should you avoid `Integer` in performance-critical loops?
5. Does `Boolean` also have caching? (Yes — `TRUE`/`FALSE` singletons)

---

## 🎯 Summary

✔ Wrapper classes = object versions of primitives, needed for Collections/Generics  
✔ Autoboxing/Unboxing = automatic primitive ↔ wrapper conversion  
✔ Integer caching (-128 to 127) means `==` can misleadingly return `true`/`false`  
✔ Always use `.equals()` for wrapper object comparison  
✔ Unboxing a `null` wrapper throws `NullPointerException`
