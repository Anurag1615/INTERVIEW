# 🚀 PART 9 – Deadlock, Race Condition, Livelock & Starvation

---

# 1️⃣ Race Condition

## 🔹 Definition

Race condition happens when:

Two or more threads access shared data  
AND  
The final result depends on execution order.

This causes unpredictable results.

---

## 🔹 Example

```java
class Counter {
    int count = 0;

    public void increment() {
        count++;
    }
}
```

If two threads run increment() at same time:

Expected:
count = 2

Possible result:
count = 1

Because:

count++ is not atomic.

---

## 🔹 Why It Happens?

count++ internally does:

1. Read value
2. Add 1
3. Write back

If two threads read same value before writing,
data gets corrupted.

---

## 🔹 Solution

✔ synchronized  
✔ AtomicInteger  
✔ Lock  

---

# 2️⃣ Deadlock

## 🔹 Definition

Deadlock happens when:

Two threads wait for each other forever.

Both are blocked.

Program never continues.

---

## 🔹 Simple Example

Thread 1:
Locks A → waits for B

Thread 2:
Locks B → waits for A

Now:

Thread 1 waits for Thread 2  
Thread 2 waits for Thread 1  

System stuck forever.

---

## 🔹 Code Example

```java
class Test {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                synchronized (lock2) {
                    System.out.println("Thread 1");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                synchronized (lock1) {
                    System.out.println("Thread 2");
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

---

## 🔹 How to Prevent Deadlock?

✔ Always lock resources in same order  
✔ Use tryLock()  
✔ Use timeout locks  
✔ Avoid nested locking  

---

# 3️⃣ Livelock

## 🔹 Definition

Livelock happens when:

Threads are not blocked  
BUT  
They keep responding to each other  
And never complete work.

They are active  
But no progress happens.

---

## 🔹 Simple Example

Two people trying to cross:

Person A moves left  
Person B moves right  

Both adjust again and again  
Still cannot cross  

This is livelock.

---

# 4️⃣ Starvation

## 🔹 Definition

Starvation happens when:

A thread never gets CPU time  
Because higher priority threads keep running.

It waits forever.

---

## 🔹 Example

If one thread has MAX_PRIORITY  
And others have low priority

Low priority threads may never execute.

---

# 🔥 Comparison Table

| Problem | What Happens | Thread State |
|----------|-------------|--------------|
| Race Condition | Wrong result | Running |
| Deadlock | Threads blocked forever | Blocked |
| Livelock | Threads active but stuck | Running |
| Starvation | Thread never gets chance | Waiting |

---

# 🎯 Interview Questions

1. What is race condition?
2. How to prevent race condition?
3. What is deadlock?
4. What are 4 conditions of deadlock?
5. Difference between deadlock and livelock?
6. What is starvation?

---

# 🧠 Extra (Important Interview Point)

## 4 Necessary Conditions for Deadlock

1. Mutual Exclusion  
2. Hold and Wait  
3. No Preemption  
4. Circular Wait  

If you break any one condition → No deadlock.

---

🔥 END OF PART 9
