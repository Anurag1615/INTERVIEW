# 🔴 CONSTRUCTORS

---

## 1️⃣ Purpose

Constructor ka purpose hai:

→ Object ko initialize karna.

---

## 2️⃣ Rules for Constructor

1. Constructor ka name same hota hai class name ke.
2. Return type concept constructor me applicable nahi hota.
3. Constructor ke liye allowed modifiers:
   - public
   - default
   - protected
   - private

---

## 3️⃣ Default Constructor

Agar class me koi constructor nahi hai  
to JVM automatically ek constructor create karta hai  
jise default constructor kehte hain.

✔ Default constructor always no-arg constructor hota hai  
❌ Har no-arg constructor default constructor nahi hota  

✔ Default constructor ka access modifier same hota hai class ke modifier ke:

- public class → public default constructor  
- default class → default constructor  

✔ Default constructor internally sirf:

    super();

contain karta hai.

---

## 4️⃣ super() & this() Rule

Constructor ki first line me ya to:

    super();

ya

    this();

hona chahiye.

Agar kuch nahi likhenge → compiler automatically super(); likh dega.

---

### Important Points

✔ super() & this() sirf constructor ke andar use ho sakte hain  
✔ Sirf first statement me use ho sakte hain  
✔ Ek constructor me dono ek saath use nahi kar sakte  

---

## 5️⃣ this & super Keyword (Variables ke liye)

Example:

```java
class P {
    String s = "parent var";
}

class C extends P {
    String s = "child var";

    public void m1() {
        System.out.println(s);        // child var
        System.out.println(this.s);   // child var
        System.out.println(super.s);  // parent var
    }
}
```

✔ this → current class instance variable  
✔ super → parent class instance variable  

❌ Static area me this & super use nahi kar sakte  

---

## 6️⃣ super() & this() vs super & this

super() & this()  
→ Constructor call karne ke liye  

super & this  
→ Variables refer karne ke liye  

---

## 7️⃣ Constructor Overloading

Ek class me multiple constructors different parameters ke saath define karna = Constructor Overloading

Example:

```java
class Enquiry {

    Enquiry(String name, long mob, String mail, String add, String course) {}

    Enquiry(String name, long mob) {}

    Enquiry(String name) {}
}
```

---

## 8️⃣ Constructor Chaining

Example:

```java
class Test {

    Test(double d) {
        this(10);
        System.out.println("double-arg constructor");
    }

    Test(int i) {
        this();
        System.out.println("int-arg constructor");
    }

    Test() {
        System.out.println("no-arg constructor");
    }

    public static void main(String[] args) {

        Test t1 = new Test(10.5);
        Test t2 = new Test(10);
        Test t3 = new Test();
    }
}
```

---

## 9️⃣ Important Concept

Inheritance & Overriding constructor ke liye available nahi hai.

Constructor inherit nahi hota.  
Constructor override nahi hota.

---

## 🔟 Interface and Constructor

Interface me constructor nahi hota.

Reason:

Interface ke variables hamesha:
    public static final hote hain.

---

## 1️⃣1️⃣ Abstract Class and Constructor

Abstract class me constructor allowed hai.

Example:

```java
abstract class Test {

    int x;

    Test() {
        System.out.println("Abstract class constructor");
    }
}
```

✔ Ye constructor tab execute hota hai jab child class ka object create hota hai.

---

## 1️⃣2️⃣ Recursive Constructor Problem

Agar constructor indirectly khud ko call kare:

```java
class P {
    P() {
        new C();
    }
}

class C extends P {
    C() {
        new P();
    }
}
```

Result:
→ StackOverflowError

---

## 1️⃣3️⃣ Recursive Constructor Invocation

```java
class P {
    P(int i) {}
}

class C extends P {
    C() {
        super();
    }
}
```

❌ Compile Time Error

Reason:
Parent class me no-arg constructor nahi hai.

---

## 1️⃣4️⃣ Checked Exception in Constructor

```java
class P {
    P() throws IOException {}
}

class C extends P {
    C() throws IOException {
        super();
    }
}
```

✔ Agar parent constructor checked exception throw karta hai  
to child constructor ko bhi same exception handle ya declare karna padega.

---

## 🔥 Final Summary

✔ Constructor name = class name  
✔ No return type  
✔ super() / this() first statement  
✔ Only one allowed  
✔ Constructor inherit nahi hota  
✔ Constructor override nahi hota  
✔ Interface me constructor nahi hota  
✔ Abstract class me constructor allowed  
✔ Recursive constructor → StackOverflowError  
✔ Parent no-arg constructor missing → Compile Error  
