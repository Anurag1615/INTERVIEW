# 📘 Java Core Concepts 

---

# 1️⃣ Java Source File Structure

## 🔹 Rule 1

A Java source file can contain:

- Multiple classes
- But at most ONE public class

---

## 🔹 Rule 2

If a public class is present  
→ File name must match the public class name.

### Example

```java
class A { }
class B { }
public class C { }
class D { }
```

✔ File name must be:

```
C.java
```

---

## 🔹 Important Points

- If no public class → file name can be anything.
- If public class present → file name must match exactly.

---

# 2️⃣ Import Statement
An import statement is a directive used to bring external code—such as classes, functions, or modules—into your current file

## 🔹 Fully Qualified Name

Without import:

```java
java.util.ArrayList list = new java.util.ArrayList();
```

✔ Valid  
❌ Not readable if used multiple times  

---

## 🔹 Using Import

```java
import java.util.ArrayList;
```

Then we can write:

```java
ArrayList list = new ArrayList();
```

✔ Cleaner  
✔ Readable  

---

## 🔹 Types of Import

### 1️⃣ Explicit Import (Recommended)

```java
import java.util.ArrayList;
```

✔ Better readability  

---

### 2️⃣ Wildcard Import

```java
import java.util.*;
```

✔ Imports all classes of package  
❌ Not recommended for large projects  

---

# 3️⃣ Package Statement
A package statement is a keyword in Java used  to define  a group of related classes and interfaces
## 🔹 Position in File

Order must be:

1. Package statement  
2. Import statement  
3. Class declaration  

---

## 🔹 Example

```java
package pk1;

import java.util.*;

public class Test {
}
```

✔ Package statement must be first line (if present)

---

# 4️⃣ Class Level Modifiers (Top-Level Class)

## ✅ Allowed Modifiers

- public
- default
- abstract
- final

## ❌ Not Allowed

- private
- protected
- static

> Note: private/protected/static are allowed only for inner classes.

---

# 5️⃣ Abstract Method

## 🔹 Definition

- Only declaration
- No implementation
- Must be inside abstract class

### Example

```java
abstract class Vehicle {
    public abstract int getNoOfWheels();
}
```

✔ Method body not allowed.

---

# 6️⃣ Abstract Class

## 🔹 Definition

- Partially implemented class
- Object creation not allowed

### Example

```java
abstract class Test {
}
```

```java
Test t = new Test();   // ❌ Compile Time Error
```

---

## 🔹 Important Rule

If a class contains even ONE abstract method  
→ Class must be declared abstract.

---

## 🔹 Incomplete Implementation Example

```java
abstract class Test {
    public abstract void m1();
    public abstract void m2();
}

class SubTest extends Test {
    public void m1() {
    }
}
```

❌ Compile Error  
Because m2() not implemented.

---

## ✔ Solution 1: Implement all methods

```java
class SubTest extends Test {
    public void m1() {}
    public void m2() {}
}
```

---

## ✔ Solution 2: Declare subclass abstract

```java
abstract class SubTest extends Test {
}
```

---

# 7️⃣ Member Modifiers

---
Access modifiers define how the members of a class, like variables, methods, and even the class itself, can be accessed from other parts of our program.

## 🔹 Public Member

⚠ First class modifier is checked.

If class is not public  
→ Even if a method inside that class is marked public, code in a different package cannot call it because it cannot access the class to create an instance or reference it.
Inside the same package, the member behaves as truly public.

---

## 🔹 Private Member

- Accessible only within same class
- Not inherited

### Example

```java
class A {
    private void m1() {
        System.out.println("Private method");
    }
}

class Test {
    public static void main(String[] args) {
        A a = new A();
        a.m1();   // ❌ Compile Time Error
    }
}
```

---

## 🔹 Recommended Practice

✔ Variables → private  
✔ Methods → public  

---

# 8️⃣ Protected Modifier

## 🔹 Definition

- Same package → Accessible anywhere
- Different package → Only accessible in child class

### Shortcut Formula

```
protected = default + child
```

---

## 📌 Same Package Example

```java
package pack1;

public class A {
    protected void m1() {
        System.out.println("A class protected method");
    }
}

class B extends A {
    public static void main(String[] args) {

        A a = new A();
        a.m1();   // ✔ Accessible

        B b = new B();
        b.m1();   // ✔ Accessible

        A a1 = new B();
        a1.m1();  // ✔ Accessible
    }
}
```

---

## 📌 Different Package Example

```java
package pack2;

import pack1.A;

public class B extends A {

    public static void main(String[] args) {

        A a = new A();
        a.m1();   // ❌ Compile Time Error

        B b = new B();
        b.m1();   // ✔ Accessible

        A a1 = new B();
        a1.m1();  // ❌ Compile Time Error
    }
}
```

---

## 🔥 Important Rule

Different package me:

- Parent reference → ❌ Not allowed
- Child reference → ✔ Allowed

---

# 📌 Access Modifier Summary Table

| Modifier   | Same Class | Same Package | Child (Diff Package) | Others |
|------------|------------|--------------|----------------------|--------|
| private    | ✔          | ❌           | ❌                   | ❌     |
| default    | ✔          | ✔            | ❌                   | ❌     |
| protected  | ✔          | ✔            | ✔                    | ❌     |
| public     | ✔          | ✔            | ✔                    | ✔     |
