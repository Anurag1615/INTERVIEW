# 🔹 SOLID Principles

---

## 🔹 Why SOLID?

SOLID is a set of 5 object-oriented design principles that help write code that is:

- Maintainable
- Extensible
- Testable
- Loosely coupled

Coined by Robert C. Martin (Uncle Bob). Extremely common in Java/design interviews.

---

## 1️⃣ S — Single Responsibility Principle (SRP)

**A class should have only ONE reason to change** — i.e., it should do exactly one job.

### ❌ Violation

```java
class Employee {
    void calculateSalary() { }
    void saveToDatabase() { }   // separate responsibility
    void generateReport() { }   // separate responsibility
}
```

### ✔ Fixed

```java
class Employee {
    void calculateSalary() { }
}

class EmployeeRepository {
    void saveToDatabase(Employee e) { }
}

class ReportGenerator {
    void generateReport(Employee e) { }
}
```

---

## 2️⃣ O — Open/Closed Principle (OCP)

**Classes should be open for extension, but closed for modification.**  
Add new functionality by adding new code, not by changing existing tested code.

### ❌ Violation

```java
class DiscountService {
    double getDiscount(String customerType) {
        if (customerType.equals("REGULAR")) return 0.1;
        else if (customerType.equals("PREMIUM")) return 0.2;
        // adding new type requires modifying this method
        return 0;
    }
}
```

### ✔ Fixed (using polymorphism)

```java
interface Discount {
    double getDiscount();
}

class RegularCustomerDiscount implements Discount {
    public double getDiscount() { return 0.1; }
}

class PremiumCustomerDiscount implements Discount {
    public double getDiscount() { return 0.2; }
}
// New customer type = new class, no existing code touched
```

---

## 3️⃣ L — Liskov Substitution Principle (LSP)

**Objects of a subclass should be replaceable with objects of the superclass without breaking the application.**

### ❌ Classic Violation

```java
class Bird {
    void fly() { System.out.println("Flying"); }
}

class Ostrich extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException("Ostrich can't fly!");
    }
}
```

`Ostrich` is a `Bird` but breaks the contract expected from `Bird` (fly() should work).

### ✔ Fixed (better hierarchy)

```java
interface Bird { }

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() { System.out.println("Flying"); }
}

class Ostrich implements Bird {
    // no fly() forced on it
}
```

---

## 4️⃣ I — Interface Segregation Principle (ISP)

**Clients should not be forced to depend on methods they do not use.**  
Prefer many small, specific interfaces over one large "fat" interface.

### ❌ Violation

```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() { }
    public void eat() {
        // Robots don't eat! Forced to implement unused method
        throw new UnsupportedOperationException();
    }
}
```

### ✔ Fixed

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() { }
    public void eat() { }
}

class Robot implements Workable {
    public void work() { }
}
```

---

## 5️⃣ D — Dependency Inversion Principle (DIP)

**High-level modules should not depend on low-level modules; both should depend on abstractions.**

### ❌ Violation

```java
class MySQLDatabase {
    void save(String data) { }
}

class UserService {
    private MySQLDatabase db = new MySQLDatabase(); // tightly coupled
    void saveUser(String data) {
        db.save(data);
    }
}
```

### ✔ Fixed (depend on abstraction)

```java
interface Database {
    void save(String data);
}

class MySQLDatabase implements Database {
    public void save(String data) { }
}

class UserService {
    private Database db; // depends on abstraction, not implementation

    UserService(Database db) { // Dependency Injection
        this.db = db;
    }

    void saveUser(String data) {
        db.save(data);
    }
}
```

This makes `UserService` reusable with any `Database` implementation (MySQL, Mongo, in-memory for tests, etc.) — the exact idea behind Spring's Dependency Injection.

---

## 🔹 Quick Recap Table

| Principle | One-liner |
|-----------|-----------|
| **S**RP | One class → one responsibility |
| **O**CP | Open for extension, closed for modification |
| **L**SP | Subtypes must be substitutable for base types |
| **I**SP | Many small interfaces > one fat interface |
| **D**IP | Depend on abstractions, not concrete classes |

---

## 🔥 Interview Questions

1. Explain SOLID with real-world examples for each principle.
2. How does SRP differ from ISP? (SRP is about classes, ISP is about interfaces)
3. How does Spring Framework apply the Dependency Inversion Principle?
4. Give an example where violating LSP causes a runtime bug.
5. Why does OCP reduce regression bugs in large codebases?

---

## 🎯 Summary

✔ SOLID principles improve maintainability and reduce coupling  
✔ Followed heavily in frameworks like Spring (DI is DIP in action)  
✔ Frequently tested through code-smell / refactoring interview questions
