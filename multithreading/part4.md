# 🚀 JAVA MULTITHREADING – PART 4 (Locks & Advanced Synchronization)

---

# 1️⃣ Why Do We Need Locks?

Problem with synchronized:

❌ No flexibility  
❌ No tryLock  
❌ No timeout  
❌ No fairness policy  
❌ No separate read/write control  

Solution:
Use Lock API (java.util.concurrent.locks)

---

# 2️⃣ ReentrantLock

## 🔹 What is ReentrantLock?

Advanced alternative of synchronized.

Reentrant means:
Same thread can acquire same lock multiple times.

---

## 🔹 Basic Example

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Counter {

    private int count = 0;
    private Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        return count;
    }
}
```

---

## 🔹 Why Use try-finally?

If exception occurs → unlock must happen  
Otherwise deadlock.

Always:

```
lock.lock();
try {
   // critical section
} finally {
   lock.unlock();
}
```

---

# 3️⃣ tryLock()

Unlike synchronized, Lock supports:

```java
if(lock.tryLock()) {
    try {
        // critical section
    } finally {
        lock.unlock();
    }
}
```

✔ Non-blocking  
✔ Useful in deadlock prevention  

---

## 🔹 tryLock with timeout

```java
lock.tryLock(2, TimeUnit.SECONDS);
```

Thread waits max 2 seconds.

---

# 4️⃣ Fair Lock

By default:
Lock is non-fair.

Fair Lock:
Threads get lock in order of request.

```java
Lock lock = new ReentrantLock(true);
```

true → fairness enabled  

---

# 5️⃣ synchronized vs ReentrantLock

| synchronized | ReentrantLock |
|--------------|---------------|
| Keyword | Class |
| Less flexible | More flexible |
| No tryLock | Supports tryLock |
| No timeout | Supports timeout |
| No fairness | Supports fairness |
| Automatic unlock | Manual unlock |

---

# 6️⃣ ReadWriteLock

Problem:
Multiple threads only reading  
But synchronized allows only one thread.

Solution:
ReadWriteLock

---

## 🔹 Example

```java
import java.util.concurrent.locks.*;

class Data {

    private int value = 0;
    private ReadWriteLock rwLock = new ReentrantReadWriteLock();

    public void read() {
        rwLock.readLock().lock();
        try {
            System.out.println("Reading: " + value);
        } finally {
            rwLock.readLock().unlock();
        }
    }

    public void write(int newValue) {
        rwLock.writeLock().lock();
        try {
            value = newValue;
        } finally {
            rwLock.writeLock().unlock();
        }
    }
}
```

---

## 🔹 Rule

✔ Multiple readers allowed  
❌ Only one writer allowed  

---

# 7️⃣ StampedLock (Advanced)

Supports:

1. Read Lock  
2. Write Lock  
3. Optimistic Lock  

---

## 🔹 Example

```java
import java.util.concurrent.locks.StampedLock;

class Data {

    private int value = 0;
    private StampedLock lock = new StampedLock();

    public void write(int newValue) {
        long stamp = lock.writeLock();
        try {
            value = newValue;
        } finally {
            lock.unlockWrite(stamp);
        }
    }

    public int read() {
        long stamp = lock.tryOptimisticRead();
        int current = value;

        if(!lock.validate(stamp)) {
            stamp = lock.readLock();
            try {
                current = value;
            } finally {
                lock.unlockRead(stamp);
            }
        }

        return current;
    }
}
```

✔ Optimistic locking improves performance  
✔ Best for read-heavy systems  

---

# 8️⃣ Semaphore

Controls number of threads accessing resource.

---

## 🔹 Example

```java
import java.util.concurrent.Semaphore;

class Test {

    static Semaphore semaphore = new Semaphore(3);

    public static void main(String[] args) {

        Runnable task = () -> {
            try {
                semaphore.acquire();
                System.out.println(Thread.currentThread().getName() + " acquired");
                Thread.sleep(2000);
            } catch(Exception e) {
            } finally {
                semaphore.release();
            }
        };

        for(int i = 0; i < 6; i++) {
            new Thread(task).start();
        }
    }
}
```

Only 3 threads allowed at same time.

---

# 9️⃣ Condition (Advanced wait/notify)

Used with Lock.

Replacement of:

wait() → await()  
notify() → signal()  

---

## 🔹 Example

```java
import java.util.concurrent.locks.*;

class Shared {

    private Lock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();

    public void waitMethod() throws InterruptedException {
        lock.lock();
        try {
            condition.await();
        } finally {
            lock.unlock();
        }
    }

    public void signalMethod() {
        lock.lock();
        try {
            condition.signal();
        } finally {
            lock.unlock();
        }
    }
}
```

✔ More flexible than wait/notify  
✔ Can create multiple conditions  

---

# 🔟 Interview Questions (Part 4)

1. Difference between synchronized and Lock?
2. What is ReentrantLock?
3. What is fairness in lock?
4. What is tryLock?
5. When to use ReadWriteLock?
6. What is StampedLock?
7. Difference between wait/notify and await/signal?
8. What is Semaphore?
9. Does Lock automatically release?

Answer:
❌ No, must call unlock() manually.

---

# 🎯 Key Takeaways

✔ ReentrantLock is advanced synchronized  
✔ tryLock helps prevent deadlock  
✔ ReadWriteLock improves performance  
✔ StampedLock best for read-heavy systems  
✔ Semaphore controls concurrent access  
✔ Condition replaces wait/notify  

---

🔥 END OF PART 4
