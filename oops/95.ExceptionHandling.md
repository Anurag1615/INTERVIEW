
# 🔴 Exception Handling

---

# 1️⃣ Definition

"Unexpected unwanted event that disturbs the normal flow of program is called Exception."

---

# 2️⃣ Objective

Graceful termination of program.

👉 We should not lose anything.  
👉 Program abnormal terminate nahi hona chahiye.

Exception Handling does not mean repairing the exception.  
We must provide alternative way to continue the program normally.

---

# 3️⃣ Runtime Problem

Most of the time exceptions are caused by our program.

Example:
- Dividing by zero
- Accessing null
- Invalid input

If exception not handled → JVM stops program.

If handled → Program continues normally.

---

# 4️⃣ Throwable Hierarchy

```
                Throwable
                    |
        -------------------------
        |                       |
     Exception                 Error
        |
   -----------------------
   |                     |
Checked              Unchecked
(Fully Checked)     (Partially Checked)
```

---

# 5️⃣ Error

Errors are not caused by our program.  
They are unrecoverable.

Example:
- OutOfMemoryError
- StackOverflowError
- VirtualMachineError

We should not handle Errors.

---

# 6️⃣ Exception

Exceptions are caused by our program.

They are recoverable.

---

# 7️⃣ Checked vs Unchecked Exception

## ✔ Fully Checked Exception

If child class throws checked exception  
Parent class method must throw same exception or its parent.

Examples:
- IOException
- SQLException
- FileNotFoundException

Compiler checking is mandatory.

---

## ✔ Partially Checked Exception (Runtime Exception)

Compiler checking not mandatory.

Examples:
- ArithmeticException
- NullPointerException
- ClassCastException
- IndexOutOfBoundsException

---

# 8️⃣ try – catch

```
try {
    risky code
}
catch(Exception e) {
    handling code
}
```

Risky code = Code which may generate exception.

Handling code = Code which gives alternative flow.

---

# 9️⃣ Various Combinations of try-catch-finally

✔ try without catch → ❌ C.E  
✔ try without finally → ❌ C.E  
✔ catch without try → ❌ C.E  
✔ finally without try → ❌ C.E  

Valid combinations:

✔ try + catch  
✔ try + finally  
✔ try + catch + finally  

---

# 🔟 finally Block

finally block always executes  
Whether exception occurs or not.

Used for cleanup activity:

- Closing file
- Closing DB connection
- Releasing resources

Example:

```java
try {
    System.out.println(10/0);
}
catch(ArithmeticException e) {
    System.out.println("Handled");
}
finally {
    System.out.println("Cleanup code");
}
```

finally always executes.

---

# 1️⃣1️⃣ finalize() Method

finalize() is method of Object class.

It is executed by Garbage Collector  
just before destroying object.

Used for cleanup activity of object.

Example:

```java
class Test {

    protected void finalize() {
        System.out.println("Object destroyed");
    }

    public static void main(String[] args) {

        Test t = new Test();
        t = null;
        System.gc();
    }
}
```

Note:
- finalize() is called by GC.
- Not guaranteed when it executes.
- It is not related to finally block.

---

# 1️⃣2️⃣ throw Keyword

throw is used to manually create exception.

Example:

```java
class Test {

    public static void main(String[] args) {

        throw new ArithmeticException("Invalid");
    }
}
```

We can throw:
- Checked exception
- Unchecked exception

But for checked exception → must handle or declare.

---

# 1️⃣3️⃣ throws Keyword

throws is used in method declaration.

It tells JVM that method may throw exception.

Example:

```java
class Test {

    public static void m1() throws Exception {
        System.out.println("Hello");
    }

    public static void main(String[] args) throws Exception {
        m1();
    }
}
```

throws → propagation  
throw → creation

---

# 1️⃣4️⃣ Difference Between throw and throws

| throw | throws |
|--------|---------|
| Used inside method | Used in method declaration |
| Used to create exception | Used to declare exception |
| Only one exception at a time | Multiple exceptions allowed |

---

# 1️⃣5️⃣ Rules in Overriding with Exceptions

If parent method throws checked exception  
Child method:

✔ Can throw same exception  
✔ Can throw child exception  
✔ Can throw no exception  

❌ Cannot throw parent of parent exception  

Unchecked exception → No restriction

---

# 1️⃣6️⃣ Multiple Catch Block

Order must be:

Child → Parent

Wrong:

```java
catch(Exception e)
catch(ArithmeticException e)
```

Correct:

```java
catch(ArithmeticException e)
catch(Exception e)
```

---

# 1️⃣7️⃣ Important Points

✔ finally block always executes  
✔ finalize() executed by GC  
✔ throw → manually create  
✔ throws → declare exception  
✔ Error should not be handled  
✔ Checked → compiler check  
✔ Unchecked → runtime  

---


# 🔴 final Keyword

final keyword applicable for:

1️⃣ Variable  
2️⃣ Method  
3️⃣ Class  

---

# 1️⃣ final Variable

If a variable is declared as final → we cannot reassign it.

Example:

```java
class Test {

    public static void main(String[] args) {

        final int x = 10;

        x = 20;   // ❌ C.E (cannot reassign)
    }
}
```

✔ Value cannot be changed  
✔ Must be initialized once  

---

## final Instance Variable

Must be initialized:

- At declaration
- Inside constructor

Example:

```java
class Test {

    final int x;

    Test() {
        x = 10;
    }
}
```

---

## final Static Variable

Must be initialized:

- At declaration
- Inside static block

Example:

```java
class Test {

    final static int x;

    static {
        x = 100;
    }
}
```

---

# 2️⃣ final Method

If a method is declared as final → it cannot be overridden.

Example:

```java
class P {

    public final void m1() {
        System.out.println("Parent");
    }
}

class C extends P {

    public void m1() {   // ❌ C.E
        System.out.println("Child");
    }
}
```

✔ Method Overriding not allowed  
✔ Inheritance allowed  
✔ Method Hiding not possible  

---

# 3️⃣ final Class

If a class is declared as final → it cannot be inherited.

Example:

```java
final class P {
}

class C extends P {   // ❌ C.E
}
```

✔ No subclass allowed  
✔ No overriding possible  

Example in Java:
- String class is final.

---

# 🔥 Important Interview Point

If method is final → cannot override  
If class is final → cannot inherit  
If variable is final → cannot reassign  

---

# 🔴 Difference Between final, finally, finalize()

| final | finally | finalize() |
|--------|---------|------------|
| Keyword | Block | Method |
| Used for restriction | Used for cleanup | Called by GC |
| Variable, Method, Class | Used in try-catch | Object class method |
| Compile-time concept | Runtime execution | Garbage Collector concept |

---

# 🔥 Summary

✔ final → Restriction  
✔ finally → Cleanup  
✔ finalize() → Before object destruction  

---

# 🎯 Complete Exception Handling Flow

Exception  
→ try  
→ catch  
→ finally  
→ throw  
→ throws  
→ final  
→ finalize()

Goal = Graceful Termination of Program

# 🔥 Final Conclusion

Exception = Runtime Problem  
Handling = Alternative Flow  

Goal = Graceful Termination of Program
