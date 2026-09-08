# 🔹 Coupling & Cohesion

---

## 🔹 Coupling

## 🔹 Definition

Coupling measures **how dependent one class/module is on another**.

👉 Goal: **Low Coupling** (Loose Coupling)

### Tight Coupling (❌ Bad)

```java
class Engine {
    void start() { System.out.println("Engine started"); }
}

class Car {
    private Engine engine = new Engine(); // Car directly creates & depends on Engine

    void drive() {
        engine.start();
    }
}
```

- `Car` is tightly bound to a specific `Engine` implementation.
- Hard to test `Car` in isolation, hard to swap `Engine` type.

### Loose Coupling (✔ Good)

```java
interface Engine {
    void start();
}

class PetrolEngine implements Engine {
    public void start() { System.out.println("Petrol engine started"); }
}

class Car {
    private Engine engine;

    Car(Engine engine) { // dependency injected from outside
        this.engine = engine;
    }

    void drive() {
        engine.start();
    }
}
```

- `Car` depends on the `Engine` abstraction, not a concrete class.
- Easy to swap implementations (`ElectricEngine`, mock for testing, etc.).

---

## 🔹 Cohesion

## 🔹 Definition

Cohesion measures **how focused and related the responsibilities within a single class/module are**.

👉 Goal: **High Cohesion**

### Low Cohesion (❌ Bad)

```java
class Utility {
    void calculateSalary() { }
    void sendEmail() { }
    void connectToDatabase() { }
    void parseJson() { }
    // too many unrelated responsibilities crammed into one class
}
```

### High Cohesion (✔ Good)

```java
class SalaryCalculator {
    void calculateSalary() { }
}

class EmailService {
    void sendEmail() { }
}

class DatabaseConnector {
    void connect() { }
}
```

Each class has a single, well-defined, focused purpose (this is essentially the Single Responsibility Principle in action).

---

## 🔹 Relationship Between Coupling, Cohesion & SOLID

- **High Cohesion** naturally leads to the **Single Responsibility Principle**.
- **Low Coupling** naturally leads to the **Dependency Inversion Principle** and easier unit testing.
- A well-designed system aims for: **High Cohesion + Low Coupling**.

---

## 🔹 Comparison Table

| Aspect | Coupling | Cohesion |
|--------|----------|----------|
| Measures | Dependency BETWEEN classes/modules | Relatedness WITHIN a class/module |
| Desired level | Low | High |
| Bad example | Class directly instantiates dependencies | Class doing many unrelated jobs |
| Improves via | Interfaces, Dependency Injection | Single Responsibility Principle |

---

## 🔥 Interview Questions

1. What is the difference between coupling and cohesion?
2. Why is loose coupling preferred in enterprise applications?
3. How does Dependency Injection help reduce coupling?
4. How would you refactor a class with low cohesion?
5. Give a real-world example of tightly coupled code and how you would fix it.

---

## 🎯 Summary

✔ Coupling → dependency between classes → aim for LOW  
✔ Cohesion → focus within a class → aim for HIGH  
✔ Low coupling + High cohesion = maintainable, testable, extensible code
