FILE — 04-Method-Reference.md
# Method Reference (Java 8)

Sometimes Lambda Expression becomes lengthy.

If lambda body only calls an existing method,
then we can replace lambda with Method Reference.

---

## Definition

Method Reference is shorthand form of Lambda Expression.

It refers an already existing method.

Symbol used:

::

(double colon operator)

---

## Example 1

### Using Lambda

```java
interface Interf {
    void m1();
}

public class Test {
    public static void main(String[] args) {
        Interf i = () -> System.out.println("Hello");
        i.m1();
    }
}
```

---

### Using Method Reference

```java
public class Test {

    public static void m2() {
        System.out.println("Hello");
    }

    public static void main(String[] args) {
        Interf i = Test::m2;
        i.m1();
    }
}
```

---

## Important Rule

Method reference argument types must match functional interface method.

Otherwise compile error.

---

## More Example

```java
interface Interf {
    public void add(int a, int b);
}

public class Test {

    public static void sum(int x, int y) {
        System.out.println(x+y);
    }

    public static void main(String[] args) {
        Interf i = Test::sum;
        i.add(10,20);
    }
}
```

---

## Instance Method Reference

```java
public class Test {

    public void m1() {
        System.out.println("Instance method");
    }

    public static void main(String[] args) {
        Test t = new Test();
        Runnable r = t::m1;
        new Thread(r).start();
    }
}
```

# Constructor Reference (Java 8)

Constructor Reference is similar to Method Reference.

Instead of referring a method, we refer a constructor.

Used when Lambda creates an object.

---

## Syntax

ClassName::new

---

## Example using Lambda

```java
interface Interf {
    public Sample get();
}

class Sample {
    Sample() {
        System.out.println("Sample Object Created");
    }
}

public class Test {
    public static void main(String[] args) {

        Interf i = () -> new Sample();

        Sample s = i.get();
    }
}
```

---

## Using Constructor Reference

```java
public class Test {
    public static void main(String[] args) {

        Interf i = Sample::new;

        Sample s = i.get();
    }
}
```

✔ Cleaner code  
✔ Recommended when object creation only

---

## Parameterized Constructor

```java
interface Interf {
    public Sample get(String name);
}

class Sample {
    Sample(String name) {
        System.out.println("Hello " + name);
    }
}

public class Test {
    public static void main(String[] args) {

        Interf i = Sample::new;
        i.get("Durga");
    }
}
```

---

## Important Rule

Functional interface method arguments  
must match constructor arguments.

Otherwise compile error.

---

## When to use?

If Lambda only creates object → use constructor reference.

---

## Difference

| Feature | Lambda | Constructor Reference |
|------|------|------|
| Object creation | () -> new Sample() | Sample::new |
| Readability | Medium | High |
| Recommended | No | Yes |


---

## Types of Method References

1. Static Method Reference  
   ClassName::staticMethod

2. Instance Method Reference (object)  
   objectRef::instanceMethod

3. Instance Method Reference (class name)  
   ClassName::instanceMethod

---

## Example (ClassName::instanceMethod)

```java
interface Interf {
    void m1(String s);
}

public class Test {

    public void display(String s) {
        System.out.println(s.toUpperCase());
    }

    public static void main(String[] args) {
        Interf i = new Test()::display;
        i.m1("durga");
    }
}
```

---

## Conclusion

Method reference improves readability.

Used when lambda only calls existing method.
