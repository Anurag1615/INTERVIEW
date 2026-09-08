# 🔹 Association, Aggregation & Composition

---

## 🔹 Why This Matters

Apart from Inheritance (**is-a** relationship), Java objects can be related through **has-a** relationships.  
These "has-a" relationships are further classified as:

- Association
- Aggregation
- Composition

Interviewers often ask this to check whether a candidate can design real-world class relationships correctly.

---

## 1️⃣ Association

## 🔹 Definition

A general relationship between two independent classes/objects.  
Neither object owns the other. Both can exist independently.

### Example

```java
class Teacher {
    String name;
}

class Student {
    String name;
    Teacher teacher; // Student is "associated" with Teacher
}
```

A `Teacher` teaches many `Student`s and a `Student` can have many `Teacher`s.  
Both objects have independent lifecycles.

---

## 2️⃣ Aggregation (Weak "Has-A")

## 🔹 Definition

A special form of Association where one class **contains** a reference to another,  
but the contained object **can exist independently** of the container.

👉 It represents a **whole-part relationship**, but the "part" can survive without the "whole".

### Example

```java
class Department {
    String name;
}

class Employee {
    String name;
    Department department; // Aggregation
}
```

If the `Employee` object is destroyed, the `Department` still exists  
(a Department can have other employees or exist with none).

---

## 3️⃣ Composition (Strong "Has-A")

## 🔹 Definition

A stronger form of Aggregation where the contained object **cannot exist independently** of the container.  
The "part" is destroyed when the "whole" is destroyed.

### Example

```java
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    // Engine is created INSIDE Car, tied to Car's lifecycle
    private final Engine engine = new Engine();

    void drive() {
        engine.start();
        System.out.println("Car is moving");
    }
}
```

- `Engine` object is created and destroyed along with `Car`.
- There's no meaningful "Engine" outside of this specific `Car` in this design.

---

## 🔹 Comparison Table

| Aspect | Association | Aggregation | Composition |
|--------|--------------|-------------|-------------|
| Relationship strength | Weak/General | Weak "has-a" | Strong "has-a" |
| Object lifecycle | Independent | Independent | Dependent (part dies with whole) |
| Ownership | None | Container doesn't own | Container owns |
| Example | Student ↔ Teacher | Employee → Department | Car → Engine |
| UML notation | Plain line | Hollow diamond | Filled diamond |

---

## 🔹 Composition vs Inheritance

| Composition | Inheritance |
|-------------|--------------|
| "has-a" relationship | "is-a" relationship |
| Achieved by holding a reference to another object | Achieved by `extends` |
| More flexible — can change behavior at runtime | Less flexible — fixed at compile time |
| Favored by design principle "**Favor Composition over Inheritance**" | Can lead to tight coupling / fragile base class problem |

### Why "Favor Composition over Inheritance"?

- Inheritance breaks encapsulation (child depends on parent's internal implementation).
- Composition allows combining behaviors flexibly (e.g., Strategy Pattern) without a rigid class hierarchy.
- Composition supports better testability (mock the composed object easily).

```java
// Composition-based flexible design
class Car {
    private Engine engine; // can be swapped: PetrolEngine, ElectricEngine, etc.

    Car(Engine engine) {
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

---

## 🔥 Interview Questions

1. What is the difference between Aggregation and Composition?
2. Give a real-world example of Association, Aggregation and Composition each.
3. Why is Composition generally preferred over Inheritance?
4. Can Aggregation exist without Association? (Aggregation is a specialized form of Association)
5. How would you model a "Car has an Engine" relationship to demonstrate Composition in code?

---

## 🎯 Summary

✔ Association → general relationship, independent lifecycles  
✔ Aggregation → weak has-a, part can exist without whole  
✔ Composition → strong has-a, part cannot exist without whole  
✔ Favor Composition over Inheritance for flexible, loosely coupled designs
