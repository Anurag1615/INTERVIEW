# Abstract Factory Design Pattern

## 🔥 Definition
> Abstract Factory Design Pattern provides an interface to create families of related or dependent objects without specifying their concrete classes.

---

# 1. Problem First (With Factory Method Limitation)

## Scenario
We need to create **multiple related objects together**.

Example:
- UI system with:
  - Button
  - Checkbox

👉 For different platforms:
- Windows
- Mac

---

## ❌ Without Abstract Factory

```java
Button btn = new WindowsButton();
Checkbox cb = new MacCheckbox(); // ❌ mismatch
```

👉 Problem:
- Mixing families (Windows + Mac) ❌  
- Inconsistent behavior  

---

# 2. Problem Summary

## ❌ 1. No Control Over Object Family
Objects may belong to different families

## ❌ 2. Tight Coupling
Client must know exact classes

## ❌ 3. Hard to Maintain Consistency

---

# 3. Solution → Abstract Factory Pattern

## Idea

👉 Create **factory of factories**  
👉 Each factory creates a **family of related objects**

```
Client → Factory → (Button + Checkbox)
```

---

# 4. Implementation

## Step 1: Abstract Products

```java
interface Button {
    void render();
}

interface Checkbox {
    void check();
}
```

---

## Step 2: Concrete Products

### Windows

```java
class WindowsButton implements Button {
    public void render() {
        System.out.println("Windows Button");
    }
}

class WindowsCheckbox implements Checkbox {
    public void check() {
        System.out.println("Windows Checkbox");
    }
}
```

---

### Mac

```java
class MacButton implements Button {
    public void render() {
        System.out.println("Mac Button");
    }
}

class MacCheckbox implements Checkbox {
    public void check() {
        System.out.println("Mac Checkbox");
    }
}
```

---

## Step 3: Abstract Factory

```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
```

---

## Step 4: Concrete Factories

```java
class WindowsFactory implements GUIFactory {

    public Button createButton() {
        return new WindowsButton();
    }

    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacFactory implements GUIFactory {

    public Button createButton() {
        return new MacButton();
    }

    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

---

## Step 5: Client

```java
class Application {

    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void renderUI() {
        button.render();
        checkbox.check();
    }
}
```

---

## Usage

```java
GUIFactory factory = new WindowsFactory(); // or MacFactory
Application app = new Application(factory);
app.renderUI();
```

---

# 5. UML Diagram

```
        +------------------+
        |      Client      |
        +------------------+
                 |
                 v
        +------------------+
        |   GUIFactory     | <<interface>>
        +------------------+
        | createButton()   |
        | createCheckbox() |
        +------------------+
           /           \
          /             \
         v               v
+----------------+   +----------------+
| WindowsFactory |   |   MacFactory   |
+----------------+   +----------------+
| createButton() |   | createButton() |
| createCheckbox()|  | createCheckbox()|
+----------------+   +----------------+
        |                    |
        v                    v
+---------------+     +---------------+
| WindowsButton |     |  MacButton    |
+---------------+     +---------------+
| render()      |     | render()      |
+---------------+     +---------------+

+------------------+
|    Checkbox      |
+------------------+
        ^
        |
+---------------------+
| WindowsCheckbox     |
+---------------------+
| check()             |
+---------------------+

+---------------------+
| MacCheckbox         |
+---------------------+
| check()             |
+---------------------+
```

---

# 6. Flow

```
Client → Abstract Factory → Concrete Factory → Family of Objects
```

---

# 7. Key Concepts (WHY)

✔ Abstract Factory → defines family creation  
✔ Concrete Factory → creates specific family  
✔ Abstract Products → common interfaces  
✔ Concrete Products → actual implementations  

---

# 8. Advantages

✔ Ensures consistency (same family)  
✔ Loose coupling  
✔ Easy to switch families  
✔ Follows Open Closed Principle  

---

# 9. Disadvantages

❌ More classes  
❌ Complex structure  

---

# 10. When to Use

✔ Multiple related objects needed  
✔ Need consistent object family  
✔ Want to switch entire system behavior  

---

# 11. Real-World Examples

- UI frameworks (Windows/Mac themes)  
- Database drivers (MySQL/Postgres families)  
- Payment systems (Stripe/PayPal full suite)  

---

# 12. Interview One-Liner

> Abstract Factory creates families of related objects without exposing their concrete classes, ensuring consistency across products.

---

# 13. Pro Tip

> Factory Method creates one object, Abstract Factory creates a family of objects.

---

# 🔥 Final Insight

```
Factory Method → One Product
Abstract Factory → Family of Products
```
