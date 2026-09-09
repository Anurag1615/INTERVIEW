# 🔹 Custom (User-Defined) Exceptions

---

## 🔹 Why Custom Exceptions?

Built-in exceptions (`ArithmeticException`, `NullPointerException`, etc.) describe generic JVM-level problems.

For **business/domain-specific errors**, we create our own exception classes to make error handling more meaningful.

Example: `InsufficientBalanceException`, `InvalidOrderException`, `UserNotFoundException`.

---

## 🔹 Creating a Custom Checked Exception

Extend `Exception` class.

```java
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}
```

Usage:

```java
class BankAccount {
    double balance = 1000;

    void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient balance for withdrawal");
        }
        balance -= amount;
    }
}

public class Test {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        try {
            acc.withdraw(5000);
        } catch (InsufficientBalanceException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

Since it extends `Exception` (checked), the compiler **forces** the caller to either handle it (`try-catch`) or declare it (`throws`).

---

## 🔹 Creating a Custom Unchecked Exception

Extend `RuntimeException`.

```java
class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);
    }
}

class Person {
    void setAge(int age) {
        if (age < 0) {
            throw new InvalidAgeException("Age cannot be negative: " + age);
        }
    }
}
```

- No `throws` declaration required.
- No compile-time forcing of `try-catch`.
- Used for programming errors / invalid input that the caller isn't required to explicitly handle.

---

## 🔹 Checked vs Unchecked Custom Exception — When to Use Which?

| Use Checked Exception When | Use Unchecked Exception When |
|------------------------------|-------------------------------|
| The caller can reasonably be expected to recover (e.g., retry, show a message) | It represents a programming bug or invalid usage |
| Business-critical failure that must not be silently ignored | Validation failures that should fail fast |
| Example: `InsufficientBalanceException`, `FileProcessingException` | Example: `InvalidAgeException`, `IllegalArgumentException`-style errors |

---

## 🔹 Best Practices

1. Always provide constructors that accept a `message` and, ideally, a `cause` (`Throwable`) for exception chaining.

```java
class OrderProcessingException extends RuntimeException {
    public OrderProcessingException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

2. Name exceptions ending with `Exception` (convention).
3. Don't create a custom exception if a suitable standard one already fits (`IllegalArgumentException`, `IllegalStateException`).
4. Keep exception classes lightweight — avoid heavy logic inside them.
5. In layered applications (e.g., Spring Boot), map custom exceptions to proper HTTP status codes via a global exception handler (`@ControllerAdvice`).

---

## 🔥 Interview Questions

1. When would you create a checked custom exception vs an unchecked one?
2. How do you preserve the original exception (root cause) when wrapping it in a custom exception?
3. Why shouldn't you overuse checked exceptions in modern API design?
4. How are custom exceptions typically handled in a Spring Boot REST API?

---

## 🎯 Summary

✔ Extend `Exception` → custom checked exception  
✔ Extend `RuntimeException` → custom unchecked exception  
✔ Always support message + cause chaining  
✔ Choose checked vs unchecked based on whether recovery is expected
