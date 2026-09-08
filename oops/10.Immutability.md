# 🔹 Immutability

---

## 🔹 Definition

An **immutable object** is an object whose state (fields) **cannot be changed** after it is created.

Example: `String`, wrapper classes (`Integer`, `Long`, etc.), `LocalDate`.

```java
String s = "hello";
s.concat(" world"); // creates a NEW string, doesn't modify original
System.out.println(s); // "hello" (unchanged)
```

---

## 🔹 Why Immutability Matters

- **Thread Safety**: Immutable objects can be shared across threads without synchronization — no thread can modify the state.
- **Caching / String Pool**: Safe to reuse and cache instances (e.g., String Pool, `Integer` cache for -128 to 127).
- **Safe as HashMap keys**: `hashCode()` never changes after insertion, so lookups always work correctly.
- **Simplicity**: No defensive copying needed when passing objects around; easier to reason about.

---

## 🔹 How to Create an Immutable Class

### Rules

1. Declare the class as `final` (prevents subclassing that could break immutability).
2. Make all fields `private` and `final`.
3. Don't provide setter methods.
4. Initialize all fields via constructor only.
5. If a field is a mutable object (like a `List` or `Date`), return a **defensive copy** from the getter — never the original reference.

### Example

```java
final class Employee {

    private final int id;
    private final String name;
    private final List<String> skills;

    public Employee(int id, String name, List<String> skills) {
        this.id = id;
        this.name = name;
        this.skills = new ArrayList<>(skills); // defensive copy on the way IN
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public List<String> getSkills() {
        return new ArrayList<>(skills); // defensive copy on the way OUT
    }
}
```

### Why the Defensive Copy Matters

```java
List<String> skills = new ArrayList<>(List.of("Java", "SQL"));
Employee e = new Employee(1, "Raj", skills);

skills.add("Python");                 // modifying original list after construction
System.out.println(e.getSkills());    // should NOT contain "Python" if truly immutable

e.getSkills().add("Kotlin");          // modifying the returned list
System.out.println(e.getSkills());    // should NOT contain "Kotlin" either
```

Without defensive copies, the "immutable" object's internal list could be silently mutated from outside — breaking immutability.

---

## 🔹 Immutability in Modern Java

- **`record` (Java 16+)** automatically generates an immutable data carrier:

```java
record Point(int x, int y) { }
// fields are private final, no setters, equals/hashCode/toString auto-generated
```

- Note: records still need defensive copies for mutable field types (like a `List`) to be *fully* immutable.

---

## 🔥 Interview Questions

1. Why is `String` immutable in Java?
2. How do you make a custom class immutable?
3. Why do we need defensive copying for mutable fields inside an immutable class?
4. Are `final` fields enough to guarantee immutability? (No — a `final` reference can still point to a mutable object whose internal state changes)
5. Why are immutable objects considered thread-safe by default?

---

## 🎯 Summary

✔ Immutable object's state can't change after construction  
✔ `final` class + `final` private fields + no setters + defensive copies  
✔ Immutability gives thread safety, safe hashing, and simpler reasoning
