# 🔹 Object Cloning

---

## 🔹 Definition

Object cloning is the process of creating an **exact copy** of an object using the `clone()` method defined in the `Object` class, instead of creating a new object manually and copying each field.

---

## 🔹 Why Cloning?

- To create a duplicate object quickly without using `new` + manual field copy.
- Useful when we need an independent copy to modify without affecting the original.

---

## 🔹 `Cloneable` Interface

```java
class Student implements Cloneable {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

- `Cloneable` is a **marker interface** (no methods).
- If a class does NOT implement `Cloneable` and `clone()` is called →  
  `CloneNotSupportedException` is thrown.

```java
Student s1 = new Student(1, "Raj");
Student s2 = (Student) s1.clone();

System.out.println(s1 == s2);        // false → different objects
System.out.println(s1.id == s2.id);  // true  → same data copied
```

---

## 🔹 Shallow Copy vs Deep Copy

### Shallow Copy (default behavior of `Object.clone()`)

- Copies **primitive fields** by value.
- Copies **reference fields** by reference (both objects point to the SAME nested object).

```java
class Address {
    String city;
    Address(String city) { this.city = city; }
}

class Student implements Cloneable {
    int id;
    Address address;

    Student(int id, Address address) {
        this.id = id;
        this.address = address;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone(); // shallow copy
    }
}

Student s1 = new Student(1, new Address("Delhi"));
Student s2 = (Student) s1.clone();

s2.address.city = "Mumbai";
System.out.println(s1.address.city); // "Mumbai" ❌ (unexpected — both share same Address)
```

👉 Changing `s2`'s nested object affected `s1` too — because both share the same `Address` reference.

### Deep Copy

- Nested/referenced objects are also cloned (recursively), so both copies are completely independent.

```java
@Override
public Object clone() throws CloneNotSupportedException {
    Student cloned = (Student) super.clone();
    cloned.address = new Address(this.address.city); // clone nested object too
    return cloned;
}
```

Now modifying `s2.address` will NOT affect `s1.address`.

---

## 🔹 Comparison

| Aspect | Shallow Copy | Deep Copy |
|--------|--------------|-----------|
| Primitive fields | Copied by value | Copied by value |
| Reference fields | Shared (same object) | Independently cloned |
| Default `clone()` | Shallow | Must implement manually |
| Performance | Faster | Slower (more object creation) |
| Independence | Not fully independent | Fully independent |

---

## 🔹 Alternatives to `clone()`

Because `Object.clone()` is considered clunky (checked exception, need for `Cloneable`, shallow-copy-by-default pitfalls), modern code often prefers:

- **Copy constructors**
  ```java
  Student(Student other) {
      this.id = other.id;
      this.address = new Address(other.address.city);
  }
  ```
- **Builder pattern** to construct a new equivalent object.
- Serialization-based deep copy (rarely used, expensive).

---

## 🔥 Interview Questions

1. What is the difference between shallow copy and deep copy?
2. What happens if `clone()` is called on a class that doesn't implement `Cloneable`?
3. Why is `Object.clone()` considered a design flaw by many (Effective Java)?
4. How do you achieve a deep copy manually?
5. Is `clone()` used commonly in real-world projects? What's preferred instead?

---

## 🎯 Summary

✔ `clone()` creates a copy of an object  
✔ Class must implement `Cloneable` marker interface  
✔ Default `clone()` → shallow copy  
✔ Deep copy requires manually cloning nested/reference fields  
✔ Copy constructors are often a cleaner alternative
