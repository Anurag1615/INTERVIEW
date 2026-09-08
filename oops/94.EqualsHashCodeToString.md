# 🔹 equals(), hashCode() & toString()

---

## 🔹 Why These Methods Matter

Every class in Java implicitly extends `Object`.  
`Object` provides three methods that are asked in almost every Java interview:

- `equals()`
- `hashCode()`
- `toString()`

---

## 1️⃣ `==` vs `equals()`

| `==` | `equals()` |
|------|------------|
| Compares **reference** (memory address) | Compares **content/state** (by default, also reference) |
| Operator | Method (defined in `Object` class) |
| Cannot be overridden | Can be overridden |
| Works for primitives too | Works only for objects |

### Example

```java
String s1 = new String("abc");
String s2 = new String("abc");

System.out.println(s1 == s2);       // false → different objects
System.out.println(s1.equals(s2));  // true  → same content
```

---

## 2️⃣ Default `equals()` Behavior

Default implementation in `Object` class:

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

👉 By default, `equals()` behaves exactly like `==` (reference comparison)  
unless the class overrides it (like `String`, `Integer`, wrapper classes).

---

## 3️⃣ Overriding `equals()`

```java
class Employee {
    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;

        Employee e = (Employee) obj;
        return id == e.id && name.equals(e.name);
    }
}
```

---

## 4️⃣ `hashCode()`

- Returns an integer (hash) representation of the object.
- Used internally by hash-based collections (`HashMap`, `HashSet`, `Hashtable`) to decide the bucket location.

Default implementation is generally derived from the object's memory address.

```java
System.out.println(new Object().hashCode()); // some int value
```

---

## 5️⃣ equals() – hashCode() Contract ⭐ (Very Important)

1. If two objects are **equal** (`equals()` returns `true`) → they **must** have the **same hashCode()**.
2. If two objects have the **same hashCode()** → they are **not required** to be equal (hash collision is allowed).
3. If `equals()` is overridden → `hashCode()` **must also be overridden** to maintain the contract.

### What Breaks If You Violate This?

```java
class Employee {
    int id;
    Employee(int id) { this.id = id; }

    @Override
    public boolean equals(Object obj) {
        return obj instanceof Employee && ((Employee) obj).id == id;
    }
    // ❌ hashCode() NOT overridden
}

Set<Employee> set = new HashSet<>();
set.add(new Employee(1));
System.out.println(set.contains(new Employee(1))); // ❌ false (bug!)
```

Because both objects are `equal` but have **different hashCodes**,  
`HashSet` looks in the wrong bucket and fails to find the duplicate.

### Correct Version

```java
@Override
public int hashCode() {
    return Objects.hash(id);
}
```

---

## 6️⃣ `toString()`

- Returns String representation of an object.
- Default implementation:

```java
getClass().getName() + "@" + Integer.toHexString(hashCode())
```

Example without override:

```java
Employee e = new Employee(1, "Raj");
System.out.println(e); // Employee@1b6d3586
```

### Overriding `toString()`

```java
@Override
public String toString() {
    return "Employee{id=" + id + ", name='" + name + "'}";
}
```

✔ Used heavily for logging and debugging.  
✔ Called implicitly by `System.out.println(obj)` and string concatenation.

---

## 🔥 Interview Questions

1. Why should `hashCode()` always be overridden along with `equals()`?
2. Can two unequal objects have the same `hashCode()`?
3. What happens if you only override `equals()` but not `hashCode()` and use the object as a `HashMap` key?
4. Is it mandatory to override `toString()`?
5. What does `Objects.equals()` and `Objects.hash()` do, and why are they preferred?

---

## 🔥 Common Mistakes

- Overriding `equals()` without overriding `hashCode()`.
- Using mutable fields in `equals()`/`hashCode()` for objects stored as `HashMap`/`HashSet` keys (their hash can change after mutation, causing lookup failures).
- Forgetting `null` check and `instanceof`/`getClass()` check inside `equals()`.

---

## 🎯 Summary

✔ `==` → reference comparison  
✔ `equals()` → logical/content comparison (override for value equality)  
✔ `hashCode()` → must be consistent with `equals()`  
✔ `toString()` → human-readable representation, override for meaningful logs
