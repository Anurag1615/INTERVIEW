# 🔹 Dynamic Method Dispatch (Runtime Polymorphism Mechanism)

---

## 🔹 Definition

Dynamic Method Dispatch is the mechanism by which a call to an **overridden method** is resolved  
**at runtime** rather than at compile time, based on the **actual object type**, not the reference type.

This is HOW runtime polymorphism actually works internally in Java.

---

## 🔹 How It Works

```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Cat meows");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a; // reference type = Animal

        a = new Dog();
        a.sound(); // "Dog barks" → decided at RUNTIME based on actual object

        a = new Cat();
        a.sound(); // "Cat meows" → decided at RUNTIME based on actual object
    }
}
```

- Reference type (`Animal a`) is fixed at **compile time**.
- Actual method that executes is decided at **runtime**, based on the object `a` currently points to.
- This is why it's also called **late binding** or **runtime binding**.

---

## 🔹 Compile-Time Check vs Runtime Decision

At **compile time**, the compiler only checks:
- Does the reference type (`Animal`) have a method called `sound()`? If yes → compiles fine.

At **runtime**, the JVM decides:
- Which actual overridden version (`Dog.sound()` or `Cat.sound()`) to execute, using the object's real class via the **virtual method table (vtable)**.

```java
Animal a = new Dog();
// a.bark(); // ❌ Compile Error — Animal reference doesn't know about bark()
```

Even though the actual object is a `Dog`, you cannot call `Dog`-specific methods through an `Animal` reference unless you downcast.

---

## 🔹 Static Binding vs Dynamic Binding

| Aspect | Static Binding | Dynamic Binding |
|--------|-----------------|------------------|
| Resolved at | Compile time | Runtime |
| Applies to | `private`, `static`, `final` methods, overloaded methods | Overridden (instance) methods |
| Also called | Early binding | Late binding |
| Based on | Reference type | Actual object type |

### Example of Static Binding (won't dispatch dynamically)

```java
class Parent {
    static void show() {
        System.out.println("Parent static method");
    }
}

class Child extends Parent {
    static void show() {
        System.out.println("Child static method");
    }
}

Parent p = new Child();
p.show(); // "Parent static method" → static methods are NOT dynamically dispatched (resolved by reference type)
```

`static` methods belong to the class, not the object — so they are resolved via the **reference type** at compile time, not through dynamic dispatch.

---

## 🔥 Interview Questions

1. What is Dynamic Method Dispatch and how is it different from method overloading resolution?
2. Why are `static` methods not dynamically dispatched?
3. What determines which overridden method gets called at runtime?
4. What is a vtable / virtual method table conceptually?
5. Can `private` methods be dynamically dispatched? (No — they're resolved statically, they aren't inherited/overridden)

---

## 🎯 Summary

✔ Dynamic Method Dispatch = runtime resolution of overridden methods  
✔ Based on actual object type, not reference type  
✔ Enables runtime polymorphism  
✔ Does NOT apply to `static`, `private`, or `final` methods (those use static binding)
