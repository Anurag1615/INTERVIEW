# 🚀 JAVA MULTITHREADING – PART 2 (Thread Creation & Lifecycle)

---

# 1️⃣ Ways to Create Thread

Java me thread create karne ke 2 main tareeke hain:

1. Extending Thread class
2. Implementing Runnable interface (Recommended)

---

# 2️⃣ Extending Thread Class

## 🔹 Example

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Thread is running");
    }

    public static void main(String[] args) {

        MyThread t = new MyThread();
        t.start();  // start new thread

        System.out.println("Main thread running");
    }
}
```

---

## 🔹 Important Point

Never call run() directly.

```java
t.run();   // ❌ This will NOT create new thread
```

✔ run() → normal method call  
✔ start() → new thread creation + run() internally called  

---

## 🔹 What Happens Internally?

When you call:

```
t.start();
```

JVM does:

1. Register thread
2. Allocate stack
3. Move thread to Runnable state
4. Call run()

---

# 3️⃣ Implementing Runnable (Recommended)

## 🔹 Why Recommended?

Because Java does NOT support multiple inheritance of classes.

If you extend Thread, you cannot extend another class.

Better approach:

```java
class MyRunnable implements Runnable {

    @Override
    public void run() {
        System.out.println("Runnable thread running");
    }

    public static void main(String[] args) {

        MyRunnable r = new MyRunnable();
        Thread t = new Thread(r);

        t.start();
    }
}
```

---

## 🔹 Using Lambda (Java 8+)

```java
Thread t = new Thread(() -> {
    System.out.println("Thread using lambda");
});

t.start();
```

---

# 4️⃣ Difference: Thread vs Runnable

| Thread Class | Runnable Interface |
|--------------|--------------------|
| Extends Thread | Implements Runnable |
| No multiple inheritance | Supports multiple inheritance |
| Tightly coupled | Loosely coupled |
| Not recommended | Recommended |

---

# 5️⃣ Thread Lifecycle

Thread ke 5 major states hote hain:

```
        NEW
         |
         v
      RUNNABLE
         |
         v
       RUNNING
        /   \
       v     v
   WAITING  BLOCKED
        \     /
         v   v
      TERMINATED
```

---

## 🔹 Explanation of States

1. NEW  
   → Thread object created but start() not called.

2. RUNNABLE  
   → start() called, waiting for CPU.

3. RUNNING  
   → CPU executing thread.

4. WAITING / BLOCKED  
   → sleep(), join(), I/O, synchronized waiting.

5. TERMINATED  
   → run() method completed.

---

# 6️⃣ Important Thread Methods

---

## 🔹 1. sleep()

Pauses current thread for given time.

```java
try {
    Thread.sleep(2000);  // 2 seconds
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

Important:
- Does NOT release lock
- Static method

---

## 🔹 2. join()

Thread.join() is used to make one thread Waits for another thread to finish.

if thread A call B.join() , thread A pause until thread B finished 
```java
class Test {

    public static void main(String[] args) throws InterruptedException {

        Thread t = new Thread(() -> {
            for(int i=1; i<=5; i++) {
                System.out.println("Child: " + i);
            }
        });

        t.start();

        t.join();  // main waits

        System.out.println("Main finished");
    }
}
```

Output:
Child prints first  
Then main prints  

---

## 🔹 3. yield()

Gives hint to scheduler that the current thread is willing to pause and let other threads of equal priority run.

current thread move from running to Runnable 

```java
Thread.yield();
```

Meaning:
"CPU, if someone else is waiting, you can execute them."

Note:
OS dependent.

---

## 🔹 4. setPriority()

Priority range:

1 (MIN_PRIORITY)
5 (NORM_PRIORITY default)
10 (MAX_PRIORITY)

```java
t.setPriority(10);
```

Note:
Priority scheduling is OS dependent.

---

# 7️⃣ Example: Multiple Threads Running

```java
class Demo {

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            for(int i=1; i<=5; i++)
                System.out.println("Thread-1: " + i);
        });

        Thread t2 = new Thread(() -> {
            for(int i=1; i<=5; i++)
                System.out.println("Thread-2: " + i);
        });

        t1.start();
        t2.start();
    }
}
```

Output unpredictable:
Because thread scheduling depends on CPU.

---

# 8️⃣ Important Interview Questions (Part 2)

1. Difference between start() and run()?
2. Why Runnable is preferred?
3. Explain thread lifecycle.
4. Does sleep() release lock?
5. What is join()?
6. Can we restart a thread?

Answer:
❌ No, once thread is terminated, cannot restart.

---

# 9️⃣ Key Takeaway

✔ start() creates new thread  
✔ run() is just method  
✔ Runnable is better than Thread  
✔ Thread lifecycle important  
✔ Output order unpredictable  

---

🔥 END OF PART 2
