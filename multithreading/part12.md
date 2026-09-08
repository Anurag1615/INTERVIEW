# 🚀 PART 12 – CountDownLatch & CyclicBarrier (Easy Explanation)

---

# 1️⃣ CountDownLatch

## 🔹 Definition

`CountDownLatch` allows one or more threads to **wait** until a set of operations being performed by other threads completes.

Think of it as a **counter** that starts at N — threads wait until the counter reaches 0.

---

## 🔹 Key Methods

```java
CountDownLatch latch = new CountDownLatch(3); // count = 3

latch.countDown(); // decrements count by 1
latch.await();      // blocks calling thread until count == 0
```

---

## 🔹 Example — Main Thread Waits for 3 Worker Threads

```java
class Worker implements Runnable {
    CountDownLatch latch;

    Worker(CountDownLatch latch) {
        this.latch = latch;
    }

    public void run() {
        System.out.println(Thread.currentThread().getName() + " working...");
        latch.countDown(); // signal completion
    }
}

public class Test {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch latch = new CountDownLatch(3);

        for (int i = 0; i < 3; i++) {
            new Thread(new Worker(latch)).start();
        }

        latch.await(); // main thread waits until count reaches 0
        System.out.println("All workers finished. Main thread proceeds.");
    }
}
```

---

## 🔹 Important Point — Cannot Be Reset

Once `CountDownLatch` reaches 0, it **cannot be reused/reset**.  
If you need a reusable barrier, use `CyclicBarrier` instead.

---

# 2️⃣ CyclicBarrier

## 🔹 Definition

`CyclicBarrier` allows a set of threads to **wait for each other** to reach a common execution point (the "barrier") before any of them proceed further.

Unlike `CountDownLatch`, it is **reusable (cyclic)** — once all threads reach the barrier, it automatically resets for the next round.

---

## 🔹 Key Constructor & Methods

```java
CyclicBarrier barrier = new CyclicBarrier(3, () -> {
    System.out.println("All 3 threads reached the barrier!"); // optional action, runs once when barrier trips
});

barrier.await(); // each thread calls this and blocks until all 3 have called it
```

---

## 🔹 Example — 3 Threads Synchronizing at a Checkpoint

```java
class Task implements Runnable {
    CyclicBarrier barrier;

    Task(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    public void run() {
        try {
            System.out.println(Thread.currentThread().getName() + " doing work...");
            barrier.await(); // wait for other threads
            System.out.println(Thread.currentThread().getName() + " passed the barrier!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

public class Test {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("Barrier tripped!"));

        for (int i = 0; i < 3; i++) {
            new Thread(new Task(barrier)).start();
        }
    }
}
```

---

# 3️⃣ CountDownLatch vs CyclicBarrier

| Aspect | CountDownLatch | CyclicBarrier |
|--------|-----------------|----------------|
| Reusable | ❌ No (one-time use) | ✔ Yes (resets automatically) |
| Who waits | One or more threads wait for others to finish | All participating threads wait for each other |
| Triggered by | `countDown()` calls (can be called by non-participant threads too) | `await()` calls from the SAME set of participant threads |
| Optional action on completion | ❌ No | ✔ Yes (Runnable executed when barrier trips) |
| Typical use case | Main thread waits for N worker threads to finish startup tasks | N threads perform work in phases, syncing at each phase boundary |

---

# 🔥 Interview Questions

1. What is the difference between `CountDownLatch` and `CyclicBarrier`?
2. Can `CountDownLatch` be reset once its count reaches 0?
3. What happens if a thread calls `await()` on a `CyclicBarrier` but not all participant threads reach it?
4. What is the purpose of the optional `Runnable` action in `CyclicBarrier`?
5. Give a real-world use case for `CountDownLatch`.

---

# 🎯 Summary

✔ `CountDownLatch` → one-time wait for N events to complete, cannot be reused  
✔ `CyclicBarrier` → reusable synchronization point where N threads wait for each other  
✔ Both are part of `java.util.concurrent`
