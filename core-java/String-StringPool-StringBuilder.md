# String, String Pool, StringBuilder & StringBuffer

---

## 1️⃣ Why is `String` Immutable?

Once a `String` object is created, its content **cannot be changed**.

```java
String s = "hello";
s.concat(" world"); // creates a NEW String object
System.out.println(s); // "hello" — unchanged
```

### Reasons for Immutability

1. **String Pool caching** — immutability makes it safe to share the same `String` object across multiple references.
2. **Security** — Strings are used for class names, file paths, network connections, DB URLs; mutability could allow those to change after validation.
3. **Thread Safety** — immutable objects can be shared across threads without synchronization.
4. **`hashCode()` caching** — since content never changes, hash code can be computed once and cached (important since `String` is heavily used as a `HashMap` key).

---

## 2️⃣ String Pool (String Constant Pool)

A special memory area (inside the Heap, since Java 7+) that stores **unique String literals**.

```java
String s1 = "hello";
String s2 = "hello";

System.out.println(s1 == s2); // true → both point to SAME object in String Pool
```

```
String Pool
┌───────────┐
│ "hello" ◄─┼── s1
│           ◄─┼── s2   (both references point to the same pooled object)
└───────────┘
```

### `new String()` Bypasses the Pool

```java
String s3 = new String("hello"); // forces creation of a NEW object on heap (outside pool)

System.out.println(s1 == s3);        // false — different objects
System.out.println(s1.equals(s3));   // true — same content
```

### `intern()` Method

```java
String s4 = s3.intern(); // manually moves/fetches from String Pool
System.out.println(s1 == s4); // true
```

---

## 3️⃣ StringBuilder vs StringBuffer

Both are **mutable** sequences of characters (unlike `String`), used to avoid creating multiple intermediate `String` objects during concatenation.

```java
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");
System.out.println(sb); // "Hello World" — SAME object modified, no new object created
```

### Comparison

| Aspect | StringBuilder | StringBuffer |
|--------|---------------|--------------|
| Thread Safety | ❌ Not synchronized | ✔ Synchronized (thread-safe) |
| Performance | Faster | Slower (due to synchronization overhead) |
| Introduced In | Java 5 | Java 1.0 |
| Use When | Single-threaded code (most common case) | Multiple threads modify the same buffer |

### Why Not Just Use `String` for Concatenation?

```java
String result = "";
for (int i = 0; i < 10000; i++) {
    result += i; // creates a NEW String object on every iteration! O(n²) overall
}
```

Each `+=` creates a brand-new `String` object (since `String` is immutable), leading to poor performance for repeated concatenation. `StringBuilder`/`StringBuffer` modify the same internal buffer instead:

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append(i); // modifies the SAME internal char array, resizing only when needed
}
```

---

## 🔹 Comparison Summary Table

| Feature | String | StringBuilder | StringBuffer |
|---------|--------|----------------|---------------|
| Mutability | Immutable | Mutable | Mutable |
| Thread-safe | Yes (by virtue of immutability) | No | Yes |
| Performance (concatenation) | Slow (many objects) | Fast | Moderate (sync overhead) |
| Stored in String Pool | Yes (literals) | No | No |

---

## 🔥 Interview Questions

1. Why is `String` immutable in Java?
2. What is the String Constant Pool and how does `intern()` work?
3. What's the difference between `==` and `.equals()` for Strings?
4. When would you choose `StringBuffer` over `StringBuilder`?
5. Why is repeated `String` concatenation in a loop considered bad practice?

---

## 🎯 Summary

✔ `String` is immutable → enables pooling, caching hashCode, and thread safety  
✔ String Pool avoids duplicate literal objects; `new String()` bypasses it  
✔ `StringBuilder` (fast, not thread-safe) vs `StringBuffer` (thread-safe, slower)  
✔ Use `StringBuilder`/`StringBuffer` for heavy string concatenation
