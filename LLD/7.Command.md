# 🧠 Command Design Pattern (Easy + Interview Ready)

---

## 🚀 Why You Should Care

When you directly call methods, your code becomes:

- ❌ Tightly coupled  
- ❌ Hard to extend  
- ❌ No undo/redo support  

👉 Command Pattern solves this by converting a **request into an object**

---

## 🎯 Definition (Simple)

> Command Pattern = “Convert a request into an object”

👉 This allows you to:
- Execute commands  
- Undo/Redo  
- Queue requests  
- Log operations  

---

## 🤯 Problem (Without Command Pattern)

### Example: Remote Control

```java
class RemoteControl {
    Light light;

    void pressButton() {
        light.turnOn();
    }
}
```

### ❌ Problems:

- Remote is tightly coupled to Light  
- Cannot reuse for other devices  
- Cannot support undo/redo  
- Adding new device = change code  

---

## 💡 Solution (Command Pattern)

👉 Wrap the request inside a **Command Object**

Instead of:

```text
Remote → Light.turnOn()
```

Do this:

```text
Remote → Command → Light
```

---

## 🎮 Real Life Example

👉 Remote Control

- Remote doesn’t know HOW light works  
- It just executes a command  

👉 Same remote can control:
- Light  
- Fan  
- TV  

---

## 🧩 Structure

- **Command (Interface)** → `execute()`  
- **Concrete Command** → Actual implementation  
- **Receiver** → Object that performs action (Light)  
- **Invoker** → Calls command (Remote)  
- **Client** → Sets everything  

---

## 🧑‍💻 Code Implementation

---

### 1️⃣ Command Interface

```java
interface Command {
    void execute();
}
```

---

### 2️⃣ Receiver (Actual Work)

```java
class Light {

    void turnOn() {
        System.out.println("Light ON");
    }

    void turnOff() {
        System.out.println("Light OFF");
    }
}
```

---

### 3️⃣ Concrete Commands

```java
class LightOnCommand implements Command {

    private Light light;

    LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }
}
```

---

```java
class LightOffCommand implements Command {

    private Light light;

    LightOffCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOff();
    }
}
```

---

### 4️⃣ Invoker (Remote Control)

```java
class RemoteControl {

    private Command command;

    void setCommand(Command command) {
        this.command = command;
    }

    void pressButton() {
        command.execute();
    }
}
```

---

### 5️⃣ Client Code

```java
public class Main {
    public static void main(String[] args) {

        Light light = new Light();

        Command lightOn = new LightOnCommand(light);
        Command lightOff = new LightOffCommand(light);

        RemoteControl remote = new RemoteControl();

        remote.setCommand(lightOn);
        remote.pressButton();   // Light ON

        remote.setCommand(lightOff);
        remote.pressButton();   // Light OFF
    }
}
```

---

## 🔄 Execution Flow

```text
Client → Command → Receiver
        ↓
     Invoker
```

Step-by-step:

1. Client creates command  
2. Remote (Invoker) gets command  
3. Remote calls `execute()`  
4. Command calls Receiver (Light)  

---

## 🎯 Key Idea

> Invoker doesn’t know what happens internally  
> It just calls: `command.execute()`

---

## ✅ Advantages

- Loose coupling ✔️  
- Easy to add new commands ✔️  
- Undo/Redo support ✔️  
- Queue & logging support ✔️  

---

## ❌ Disadvantages

- More classes 😵  
- Slight complexity  

---

## 🧠 When to Use

- When you need undo/redo  
- When you want to decouple sender & receiver  
- When actions need to be queued/logged  

---

## 🆚 Command vs Direct Call

| Feature | Without Command | With Command |
|--------|----------------|--------------|
| Coupling | High ❌ | Low ✅ |
| Flexibility | Low ❌ | High ✅ |
| Undo/Redo | Not possible ❌ | Possible ✅ |

---

## 🆚 Command vs Mediator

| Feature | Command | Mediator |
|--------|--------|---------|
| Purpose | Encapsulate request | Manage communication |
| Focus | Action | Interaction |
| Example | Remote control | Chat room |

---

## 💥 Important Insight

👉 Command Pattern = **What to do (action)**  
👉 Mediator Pattern = **Who talks to whom**

---

## 🧠 Interview One-Liner

> “Command pattern converts a request into an object, enabling flexible execution, undo/redo, and loose coupling between sender and receiver.”

---

## 🚀 Easy Memory Trick

👉 **Remote Control = Command Pattern**  
👉 **Chat Room = Mediator Pattern**

---
