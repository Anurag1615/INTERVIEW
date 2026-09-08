# Serialization & Deserialization

---

## 🔹 Definition

**Serialization** is the process of converting a Java object's state into a **byte stream**,  
so it can be saved to a file, sent over a network, or stored in a database.

**Deserialization** is the reverse process — reconstructing the object from the byte stream.

---

## 🔹 How to Make a Class Serializable

Implement the `Serializable` marker interface (no methods to implement — it's just a signal to the JVM).

```java
import java.io.Serializable;

class Employee implements Serializable {
    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

---

## 🔹 Serializing an Object

```java
Employee emp = new Employee(1, "Raj");

try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("employee.ser"))) {
    oos.writeObject(emp);
}
```

## 🔹 Deserializing an Object

```java
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("employee.ser"))) {
    Employee emp2 = (Employee) ois.readObject();
    System.out.println(emp2.name); // "Raj"
}
```

---

## 🔹 `serialVersionUID`

A unique identifier for a serializable class, used during deserialization to verify that the sender and receiver of a serialized object have **compatible class versions**.

```java
class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    int id;
    String name;
}
```

### What Happens Without It?

- The JVM auto-generates one based on class details (fields, methods, etc.).
- If the class is later modified (e.g., a field is added) and the auto-generated ID changes,  
  deserializing an OLD serialized object with the NEW class version throws:

```
InvalidClassException: local class incompatible: stream classdesc serialVersionUID = X, local class serialVersionUID = Y
```

👉 **Best practice**: Always explicitly declare `serialVersionUID` to control version compatibility yourself.

---

## 🔹 `transient` Keyword

Fields marked `transient` are **excluded from serialization** — they are NOT saved and are restored with their **default values** upon deserialization.

```java
class User implements Serializable {
    String username;
    transient String password; // excluded from serialization
}

User u = new User();
u.username = "raj123";
u.password = "secret";

// after serialize + deserialize:
// u2.username → "raj123"
// u2.password → null (default value, not restored)
```

### Why Use `transient`?

- Sensitive data (passwords, API keys) shouldn't be persisted/transmitted.
- Fields that are derived/computed and can be recalculated (no need to store).
- Non-serializable fields (e.g., a `Thread` or `Socket` reference) — these would throw `NotSerializableException` if not marked `transient`.

---

## 🔹 Static Fields Are NOT Serialized

`static` fields belong to the **class**, not any specific object instance, so serialization (which is about object *state*) never includes them.

```java
class Counter implements Serializable {
    static int count = 0; // never serialized — belongs to the class
    int id;
}
```

---

## 🔹 Serialization with Inheritance

- If a superclass implements `Serializable`, all subclasses are automatically serializable too.
- If the superclass does **NOT** implement `Serializable`, its fields are **not serialized**; upon deserialization, the superclass's **no-arg constructor is invoked** to initialize those fields (so the superclass must have a public/protected no-arg constructor).

---

## 🔹 Custom Serialization

You can control exactly what gets serialized using `writeObject()` / `readObject()`:

```java
private void writeObject(ObjectOutputStream oos) throws IOException {
    oos.defaultWriteObject();
    // custom logic, e.g., encrypt sensitive data before writing
}

private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
    ois.defaultReadObject();
    // custom logic, e.g., decrypt data after reading
}
```

---

## 🔥 Interview Questions

1. What is the purpose of `serialVersionUID`?
2. What happens if you serialize an object and its class definition later changes without updating `serialVersionUID`?
3. Why are `transient` and `static` fields excluded from serialization?
4. What happens during deserialization if the superclass isn't `Serializable`?
5. What exception is thrown if you try to serialize an object containing a non-serializable field?

---

## 🎯 Summary

✔ Serialization → object → byte stream; Deserialization → byte stream → object  
✔ Implement `Serializable` (marker interface) to enable it  
✔ `serialVersionUID` ensures version compatibility across serialize/deserialize  
✔ `transient` fields are skipped (restored to default values); `static` fields are always skipped
