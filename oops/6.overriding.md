# 🔹 Method Overriding

## 🔹 Definition

If child class directly does not satisfy with parent class method implementation,  
then  child class  is allow to redefine that  method based on requirement such type of concept called overriding,  
then it is called **Method Overriding**.

Overriding is based on:

- Inheritance
- Same method signature
- Runtime polymorphism

---

## 🔹 Basic Example

```java
class P {
    public void property() {
        System.out.println("Cash + Gold + Land");
    }
}

class C extends P {
    public void property() {
        System.out.println("Only Bank Balance");
    }
}

class Test {
    public static void main(String[] args) {

        P p = new P();
        p.property();   // Parent

        P p1 = new C();
        p1.property();  // Child (Runtime Polymorphism)
    }
}
```

---

# 🔹 Rule of Method Overriding

## 1️⃣ Method Name

Must be same.

---

## 2️⃣ Argument List

Must be same (same method signature).

---

## 3️⃣ Return Type

Until 1.4 version → must be exactly same.

From 1.5 version → **Covariant return type allowed**.

### Example

```java
class P {
    public Object m1() {
        return null;
    }
}

class C extends P {
    public String m1() {
        return null;
    }
}
```

✔ Allowed (String is child of Object)

---

## 4️⃣ Access Modifier

Child method cannot reduce visibility.

Valid combinations:

- public → public
- protected → protected / public
- default → default / protected / public

Invalid:

- public → protected ❌
- protected → default ❌
- default → private ❌

---

## 5️⃣ final Method

final method cannot be overridden.

---

## 6️⃣ private Method

Private methods are not visible in child class.  
So overriding concept does not apply.

---

## 7️⃣ static Method

Static methods cannot be overridden.

(Explained below separately)

---

# 🔹 Overriding and Exceptions

### Rule:

If child class method throws checked exception,  
then parent class method must throw:

- Same exception OR
- Parent exception

Otherwise → Compile Time Error

---

## ✔ Case 1

```java
class P {
    public void m1() throws Exception {}
}

class C extends P {
    public void m1() {}
}
```

✔ Valid

---

## ✔ Case 2

```java
class P {
    public void m1() throws Exception {}
}

class C extends P {
    public void m1() throws IOException {}
}
```

✔ Valid (IOException is child of Exception)

---

## ❌ Invalid Case

```java
class P {
    public void m1() throws IOException {}
}

class C extends P {
    public void m1() throws Exception {}
}
```

❌ Compile Time Error

---

## 🔹 Unchecked Exceptions

No restriction for unchecked exceptions.

---

# 🔹 Overriding with Static Method

Static methods cannot be overridden.

It is called **Method Hiding**.

Example:

```java
class P {
    public static void m1() {
        System.out.println("Parent");
    }
}

class C extends P {
    public static void m1() {
        System.out.println("Child");
    }
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        p.m1();  // Parent
    }
}
```

Output:
```
Parent
```

Because static method resolution is compile-time based.

---

# 🔹 Method Hiding vs Method Overriding

| Feature | Overriding | Method Hiding |
|----------|------------|---------------|
| Method Type | Non-static | Static |
| Polymorphism | Runtime | Compile Time |
| Binding | Dynamic Binding | Static Binding |
| Based On |  method resolution is always takecare by the jvm based on the runtime object |  method resolution is always takecare by the compiler based on the reference type |

---

# 🔹 Overriding w.r.t Var-Args

Var-arg method is treated as normal array method.

Example:

```java
class P {
    public void m1(int... x) {}
}

class C extends P {
    public void m1(int[] x) {}
}
```

✔ Valid Overriding

Because internally var-arg = array.

---

# 🔹 Overriding w.r.t Variables

Variables cannot be overridden.

They can only be hidden.

Example:

```java
class P {
    String s = "Parent";
}

class C extends P {
    String s = "Child";
}

class Test {
    public static void main(String[] args) {

        P p = new C();
        System.out.println(p.s);
    }
}
```

Output:
```
Parent
```

Variable resolution is compile-time based on reference.

---

# 🔹 Comparison: Overloading vs Overriding

| Property | Overloading | Overriding |
|------------|-------------|-------------|
| Method Name | Must be same | Must be same |
| Arguments | Must be different | Must be same |
| Return Type | No restriction | Must be same (covariant allowed) |
| Access Modifier | No restriction | Cannot reduce visibility |
| Exception | No restriction | Checked rule applies |
| Polymorphism | Compile Time | Runtime |
| Binding | Static Binding | Dynamic Binding |
| Inheritance Required | No | Yes |
