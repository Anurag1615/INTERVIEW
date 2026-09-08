# 🚀 JAVA MULTITHREADING – PART 3 (Synchronization & Concurrency Problems)

---

# 1️⃣ What is Race Condition?

## 🔹 Definition

Race condition occurs when:

Multiple threads access and modify shared data at the same time  
→ Result becomes inconsistent.

---

## 🔹 Example (Without Synchronization)

```java
class Counter {

    int count = 0;

    public void increment() {
        count++;
    }
}

public class Test {

    public static void main(String[] args) throws InterruptedException {

        Counter c = new Counter();

        Thread t1 = new Thread(() -> {
            for(int i = 0; i < 1000; i++) {
                c.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for(int i = 0; i < 1000; i++) {
                c.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final Count: " + c.count);
    }
}
```

Expected Output:
```
Final Count: 2000
```

Actual Output:
```
Final Count: 1873 (or random)
```

---

## 🔹 Why It Happens?

count++ is NOT atomic.

Internally:

```
1. Read count
2. Increment
3. Write back
```

If two threads execute simultaneously → data overwrite.

---

# 2️⃣ What is Synchronization?

## 🔹 Definition

Synchronization is a mechanism that ensures:

Only one thread can access critical section at a time.

---

# 3️⃣ Synchronized Method

```java
class Counter {

    int count = 0;

    public synchronized void increment() {
        count++;
    }
}
```

✔ Lock applied on object (this).

Meaning:
Only one thread per object at a time.

---

# 4️⃣ Synchronized Block

Better performance (more controlled locking).

```java
public void increment() {
    synchronized(this) {
        count++;
    }
}
```

Or:

```java
synchronized(lockObject) {
   // critical section
}
```

---

## 🔹 Why Prefer Block Over Method?

Method locks entire method.  
Block locks only critical code.

More efficient.

---

# 5️⃣ Static Synchronization

If method is static:

```java
public static synchronized void test() {
}
```

Lock is applied on:

```
ClassName.class
```

Not on object.

---

# 6️⃣ Important Concept: Object Level Lock

If two threads use:

Different objects → no blocking  
Same object → blocking happens  

---

# 7️⃣ Deadlock

## 🔹 Definition

Deadlock happens when:

Two threads are waiting for each other’s lock forever.

---

## 🔹 Example

```java
class A {
    synchronized void methodA(B b) {
        System.out.println("Thread1 locked A");
        b.methodB();
    }
}

class B {
    synchronized void methodB() {
        System.out.println("Thread2 locked B");
    }
}
```

If:

Thread1 locks A  
Thread2 locks B  
Both wait for each other  
→ Deadlock

---

## 🔹 Deadlock Diagram

```
Thread-1 → Lock A → Waiting for B
Thread-2 → Lock B → Waiting for A
```

Forever waiting ❌

---

# 8️⃣ How to Prevent Deadlock?

✔ Always acquire locks in same order  
✔ Avoid nested locks  
✔ Use tryLock()  
✔ Reduce synchronization  

---

# 9️⃣ Inter-Thread Communication

Used when threads depend on each other.

Methods:

- wait()
- notify()
- notifyAll()

---

## 🔹 Example

```java
class Shared {

    synchronized void produce() throws InterruptedException {
        wait();
        System.out.println("Resumed");
    }

    synchronized void consume() {
        notify();
    }
}
```

Important Rules:

✔ Must be inside synchronized block  
✔ wait() releases lock  
✔ sleep() does NOT release lock  

---

# 🔟 Difference: wait() vs sleep()

| wait() | sleep() |
|--------|---------|
| Releases lock | Does NOT release lock |
| Object class method | Thread class method |
| Used in synchronization | Used for delay |
| Needs synchronized block | No need |

---

# 1️⃣1️⃣ Starvation

When a thread never gets CPU time due to priority.

---

# 1️⃣2️⃣ Livelock

Threads are active but unable to progress.

Example:
Both threads keep releasing locks for each other.

---

# 🎯 Interview Questions (Part 3)

1. What is race condition?
2. Why count++ is not atomic?
3. Difference between synchronized method and block?
4. What lock is applied in static synchronized?
5. Difference between sleep and wait?
6. What is deadlock?
7. How to prevent deadlock?
8. What is object level locking?
9. What is starvation?
10. What is livelock?

---

# 🔥 Key Takeaways

✔ Race condition = shared memory problem  
✔ synchronized ensures mutual exclusion  
✔ wait releases lock  
✔ sleep does not release lock  
✔ static synchronized locks class  
✔ Deadlock is circular waiting  

---

🔥 END OF PART 3
