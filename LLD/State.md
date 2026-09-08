# State Design Pattern

## 🔥 Definition
> State Design Pattern allows an object to change its behavior when its internal state changes, making it appear as if the object has changed its class.

---

# 1. Problem First (Without State Pattern)

## Scenario
We are building a **Media Player** with states:

- Playing  
- Paused  
- Stopped  

---

## ❌ Implementation (Bad Design)

```java
class MediaPlayer {

    private String state = "STOPPED";

    public void pressPlay() {

        if (state.equals("STOPPED")) {
            System.out.println("Start Playing");
            state = "PLAYING";

        } else if (state.equals("PAUSED")) {
            System.out.println("Resume Playing");
            state = "PLAYING";

        } else if (state.equals("PLAYING")) {
            System.out.println("Already Playing");
        }
    }

    public void pressPause() {

        if (state.equals("PLAYING")) {
            System.out.println("Paused");
            state = "PAUSED";

        } else {
            System.out.println("Cannot Pause");
        }
    }

    public void pressStop() {

        if (state.equals("PLAYING") || state.equals("PAUSED")) {
            System.out.println("Stopped");
            state = "STOPPED";

        } else {
            System.out.println("Already Stopped");
        }
    }
}
```

---

# 🚨 Problems

## ❌ 1. Too Many if-else
Hard to manage as states grow  

## ❌ 2. Violates Open Closed Principle
Adding new state → modify existing code  

## ❌ 3. Hard to Maintain
Logic scattered everywhere  

---

# 2. Solution → State Pattern

## Idea

👉 Represent each state as a **separate class**  
👉 Move behavior inside state  

```
Context → State → Behavior
```

---

# 3. Implementation

## Step 1: State Interface

```java
interface State {
    void pressPlay(MediaPlayer player);
    void pressPause(MediaPlayer player);
    void pressStop(MediaPlayer player);
}
```

---

## Step 2: Concrete States

### ▶ Playing State

```java
class PlayingState implements State {

    public void pressPlay(MediaPlayer player) {
        System.out.println("Already Playing");
    }

    public void pressPause(MediaPlayer player) {
        System.out.println("Paused");
        player.setState(new PausedState());
    }

    public void pressStop(MediaPlayer player) {
        System.out.println("Stopped");
        player.setState(new StoppedState());
    }
}
```

---

### ⏸ Paused State

```java
class PausedState implements State {

    public void pressPlay(MediaPlayer player) {
        System.out.println("Resume Playing");
        player.setState(new PlayingState());
    }

    public void pressPause(MediaPlayer player) {
        System.out.println("Already Paused");
    }

    public void pressStop(MediaPlayer player) {
        System.out.println("Stopped");
        player.setState(new StoppedState());
    }
}
```

---

### ⏹ Stopped State

```java
class StoppedState implements State {

    public void pressPlay(MediaPlayer player) {
        System.out.println("Start Playing");
        player.setState(new PlayingState());
    }

    public void pressPause(MediaPlayer player) {
        System.out.println("Cannot Pause");
    }

    public void pressStop(MediaPlayer player) {
        System.out.println("Already Stopped");
    }
}
```

---

## Step 3: Context (MediaPlayer)

```java
class MediaPlayer {

    private State state;

    public MediaPlayer() {
        state = new StoppedState();
    }

    public void setState(State state) {
        this.state = state;
    }

    public void pressPlay() {
        state.pressPlay(this);
    }

    public void pressPause() {
        state.pressPause(this);
    }

    public void pressStop() {
        state.pressStop(this);
    }
}
```

---

## Step 4: Client

```java
public class Main {

    public static void main(String[] args) {

        MediaPlayer player = new MediaPlayer();

        player.pressPlay();  // Start Playing
        player.pressPause(); // Paused
        player.pressPlay();  // Resume Playing
        player.pressStop();  // Stopped
    }
}
```

---

# 4. Flow

```
Client → Context → State → Behavior
                 ↓
            Change State
```

---

# 5. UML Diagram

```
        +------------------+
        |   MediaPlayer    |
        +------------------+
        | State state      |
        | setState()       |
        | pressPlay()      |
        | pressPause()     |
        | pressStop()      |
        +------------------+
                 |
                 v
        +------------------+
        |      State       | <<interface>>
        +------------------+
        | pressPlay()      |
        | pressPause()     |
        | pressStop()      |
        +------------------+
           /      |       \
          /       |        \
         v        v         v
+--------------+ +--------------+ +--------------+
| PlayingState | | PausedState  | | StoppedState |
+--------------+ +--------------+ +--------------+
```

---

# 6. Key Concepts (WHY)

✔ Context → holds current state  
✔ State → defines behavior  
✔ Concrete State → implements behavior  
✔ State change → runtime behavior change  

---

# 7. Advantages

✔ Eliminates if-else  
✔ Follows Open Closed Principle  
✔ Clean & maintainable  
✔ Easy to add new states  

---

# 8. Disadvantages

❌ More classes  
❌ Slight complexity  

---

# 9. When to Use

✔ Object behavior depends on state  
✔ Many conditional branches  
✔ State transitions are frequent  

---

# 10. Real-World Examples

- Media player  
- ATM machine states  
- Order status (Pending, Shipped, Delivered)  
- Traffic light system  

---

# 11. Interview One-Liner

> State pattern allows an object to change its behavior dynamically based on its internal state by delegating behavior to state objects.

---

# 🔥 Final Insight

```
If-else logic → Replace with State classes
```
