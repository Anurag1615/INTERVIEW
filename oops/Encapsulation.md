# 🔹 Encapsulation

## 🔹 Definition

Encapsulation is the process of grouping data members and corresponding methods into a single unit.

It combines data hiding and controlled access.

---

## 🔹 Encapsulation = Data Hiding + Abstraction

- Data hiding → private variables
- Abstraction → controlled access via methods

---

## 🔹 Example

```java
class Employee {

    private double salary;

    public void setSalary(double s) {
        salary = s;
    }

    public double getSalary() {
        return salary;
    }
}
```

---

## 🔹 Benefits of Encapsulation

- Improved security
- Better maintainability
- Controlled data access
- Flexibility to change implementation

---

## 🔹 Important Note

If we declare variables as private
but do not provide public methods,
then it is only data hiding.

If we provide controlled access,
then it becomes proper encapsulation.


# 🔹 Tightly Encapsulated Class

## 🔹 Definition

A class is said to be tightly encapsulated if all its instance variables are declared as private.

---

## 🔹 Rule

All instance variables must be private.

It does not matter whether methods are private or public.

---

## 🔹 Example (Tightly Encapsulated)

```java
class Account {

    private int balance;
    private String name;
}
```

All variables are private → ✔ Tightly Encapsulated

---

## 🔹 Example (Not Tightly Encapsulated)

```java
class Account {

    int balance;       // Default access
    private String name;
}
```

Since one variable is not private → ❌ Not tightly encapsulated

---

## 🔹 Important Interview Point

Tightly encapsulated class focuses only on:

Visibility of instance variables.

It does not depend on:

- Parent class variables
- Method visibility
