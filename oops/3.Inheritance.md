# 🔹 Inheritance

## 🔹 Definition

Inheritance is a mechanism in Java by which one class acquires the properties (variables) and behaviors (methods) of another class.

It promotes:

- Code reusability
- Runtime polymorphism
- Logical hierarchy

---

## 🔹 Terminology

- Parent Class → Superclass
- Child Class → Subclass
- Keyword used → `extends`

---

# 🔹 Basic Diagram (Single Inheritance)

```
        Parent
           ▲
           │
        Child
```

Child inherits all accessible members of Parent.

---

## 🔹 Syntax

```java
class Parent {

    int x = 10;

    void show() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {

    void display() {
        System.out.println("Child Method");
    }
}
```

---

# 🔹 Types of Inheritance

---

## 🔹 1️⃣ Single Inheritance

One child inherits from one parent.

```
      A
      ▲
      │
      B
```

```java
class A {}
class B extends A {}
```

---

## 🔹 2️⃣ Multilevel Inheritance

A class inherits from a class which itself inherits from another class.

```
      A
      ▲
      │
      B
      ▲
      │
      C
```

```java
class A {}
class B extends A {}
class C extends B {}
```

---

## 🔹 3️⃣ Hierarchical Inheritance

Multiple child classes inherit from one parent.

```
         A
        ▲ ▲
        │ │
        B C
```

```java
class A {}
class B extends A {}
class C extends A {}
```

---

## 🔹 4️⃣ Multiple Inheritance (Not Supported via Classes)

Java does NOT support multiple inheritance using classes.

```
      A     B
       ▲   ▲
        │   │
           C   ❌
```

```java
class A {}
class B {}
class C extends A, B {}   // Compile Error
```

### Reason:

Diamond Problem (Ambiguity).

---

# 🔹 Diamond Problem Diagram

```
        A
       ▲ ▲
       │ │
       B C
        ▲
        │
        D
```

If both B and C override same method from A, then D gets ambiguity.

To avoid this → Java does not allow multiple inheritance via classes.

---

# 🔹 super Keyword in Inheritance

## 🔹 Purpose

- Access parent class variable
- Call parent class method
- Call parent class constructor

---

## 🔹 Variable Access Example

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int x = 20;

    void show() {

        System.out.println(x);        // Child variable
        System.out.println(super.x);  // Parent variable
    }
}
```

---

## 🔹 Method Call Example

```java
class Parent {

    void show() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {

    void show() {
        System.out.println("Child Method");
    }

    void display() {
        show();        // Child method
        super.show();  // Parent method
    }
}
```

---

# 🔹 Constructor Execution in Inheritance

When child object is created:

1. Parent constructor executes first
2. Then child constructor executes

---

## 🔹 Constructor Flow Diagram

```
Object Creation → Child Constructor
                     │
                     ▼
              Parent Constructor
                     │
                     ▼
              Child Constructor Body
```

---

## 🔹 Example

```java
class Parent {

    Parent() {
        System.out.println("Parent Constructor");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child Constructor");
    }
}

public class Test {

    public static void main(String[] args) {

        Child c = new Child();
    }
}
```

Output:

```
Parent Constructor
Child Constructor
```

---

# 🔹 Important Rules of Inheritance

- Private members are not inherited.
- Constructors are not inherited.
- Final class cannot be extended.
- Final methods cannot be overridden.
- A class can extend only one class.
- Interfaces support multiple inheritance.

---

# 🔹 IS-A Relationship

Inheritance represents IS-A relationship.

Example:

If Dog extends Animal:

Dog IS-A Animal.

```
Dog  ─────►  Animal
```

---

## 🔹 Polymorphism Example

```java
Animal a = new Dog();
```

Reference type → Parent  
Object type → Child  

This supports runtime polymorphism.

---

# 🔹 Interview Important Points

- super() must be first statement in constructor.
- If parent constructor has parameters, child must call it explicitly.
- Java does not support multiple inheritance via classes.
- Method overriding works only in inheritance.
- Inheritance reduces code duplication.
