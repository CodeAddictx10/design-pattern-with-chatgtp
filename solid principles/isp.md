Alright! Let's break down the **Interface Segregation Principle (ISP)** in depth with **real-life examples** in **TypeScript** and **PHP**, side by side.

---

## **What is the Interface Segregation Principle (ISP)?**
The **ISP states** that no client should be forced to depend on methods it does not use. In simple terms:
- Large interfaces with too many methods should be split into smaller, **more specific interfaces**.
- Each interface should only have the methods that are relevant to the specific implementing class.

---

## **How Do You Notice a Bad Implementation of ISP?**
- A class **implements an interface but does not use all its methods**.
- The interface contains **unrelated methods**, forcing classes to implement unnecessary behaviors.
- Changes in the interface affect multiple classes unnecessarily, causing a **ripple effect** in the codebase.

---

## **Bad Example: Violating ISP**
Let's consider a **Restaurant Ordering System** where we initially design an interface for different types of employees. The mistake here is **creating one big interface for all employee roles**.

### **Bad TypeScript Example**
```typescript
// One large interface violating ISP
interface Employee {
  takeOrder(): void;
  cookFood(): void;
  processPayment(): void;
}

class Waiter implements Employee {
  takeOrder(): void {
    console.log("Taking order from customer.");
  }
  
  cookFood(): void {
    throw new Error("Waiters do not cook food!");
  }

  processPayment(): void {
    console.log("Processing payment for customer.");
  }
}

class Chef implements Employee {
  takeOrder(): void {
    throw new Error("Chefs do not take orders!");
  }

  cookFood(): void {
    console.log("Cooking food for the customer.");
  }

  processPayment(): void {
    throw new Error("Chefs do not process payments!");
  }
}
```
### **Bad PHP Example**
```php
<?php
// One large interface violating ISP
interface Employee {
    public function takeOrder();
    public function cookFood();
    public function processPayment();
}

class Waiter implements Employee {
    public function takeOrder() {
        echo "Taking order from customer.\n";
    }

    public function cookFood() {
        throw new Exception("Waiters do not cook food!");
    }

    public function processPayment() {
        echo "Processing payment for customer.\n";
    }
}

class Chef implements Employee {
    public function takeOrder() {
        throw new Exception("Chefs do not take orders!");
    }

    public function cookFood() {
        echo "Cooking food for the customer.\n";
    }

    public function processPayment() {
        throw new Exception("Chefs do not process payments!");
    }
}
?>
```
### **Problems in the above example:**
1. **Waiters do not cook food** → but they are forced to implement `cookFood()`.
2. **Chefs do not take orders** → but they are forced to implement `takeOrder()`.
3. **Empty or `throw new Error()` methods** indicate that the interface is too broad.

---

## **Good Example: Applying ISP Correctly**
To fix this, we split the large interface into smaller, more focused ones.

### **Good TypeScript Example**
```typescript
// Splitting the large interface into smaller, specific interfaces
interface OrderTaker {
  takeOrder(): void;
}

interface Cook {
  cookFood(): void;
}

interface Cashier {
  processPayment(): void;
}

// Now, each class implements only the relevant interface(s)
class Waiter implements OrderTaker, Cashier {
  takeOrder(): void {
    console.log("Taking order from customer.");
  }

  processPayment(): void {
    console.log("Processing payment for customer.");
  }
}

class Chef implements Cook {
  cookFood(): void {
    console.log("Cooking food for the customer.");
  }
}
```

### **Good PHP Example**
```php
<?php
// Splitting the large interface into smaller, specific interfaces
interface OrderTaker {
    public function takeOrder();
}

interface Cook {
    public function cookFood();
}

interface Cashier {
    public function processPayment();
}

// Now, each class implements only the relevant interface(s)
class Waiter implements OrderTaker, Cashier {
    public function takeOrder() {
        echo "Taking order from customer.\n";
    }

    public function processPayment() {
        echo "Processing payment for customer.\n";
    }
}

class Chef implements Cook {
    public function cookFood() {
        echo "Cooking food for the customer.\n";
    }
}
?>
```

### **Why is this better?**
✅ Each class only implements methods it actually needs.  
✅ **No unnecessary methods or `throw new Error()` implementations.**  
✅ **Adding a new role (e.g., Delivery Person)** is easier without modifying existing interfaces.

---

## **Another Real-Life Example: Smart Home Devices**
Imagine a **smart home system** that controls different types of devices. A **bad design** would force all devices to implement functionalities they don't support.

### **Bad Example: Violating ISP**
```typescript
interface SmartDevice {
  turnOn(): void;
  turnOff(): void;
  playMusic(): void;
}

class LightBulb implements SmartDevice {
  turnOn(): void {
    console.log("Light is turned on.");
  }

  turnOff(): void {
    console.log("Light is turned off.");
  }

  playMusic(): void {
    throw new Error("Light bulbs cannot play music!");
  }
}

class Speaker implements SmartDevice {
  turnOn(): void {
    console.log("Speaker is powered on.");
  }

  turnOff(): void {
    console.log("Speaker is powered off.");
  }

  playMusic(): void {
    console.log("Playing music.");
  }
}
```
### **Problems:**
1. **Light bulbs do not play music** → yet they are forced to implement `playMusic()`.
2. **Not all smart devices need the same capabilities.**

---

### **Good Example: Applying ISP Correctly**
```typescript
interface Switchable {
  turnOn(): void;
  turnOff(): void;
}

interface MusicPlayer {
  playMusic(): void;
}

class LightBulb implements Switchable {
  turnOn(): void {
    console.log("Light is turned on.");
  }

  turnOff(): void {
    console.log("Light is turned off.");
  }
}

class Speaker implements Switchable, MusicPlayer {
  turnOn(): void {
    console.log("Speaker is powered on.");
  }

  turnOff(): void {
    console.log("Speaker is powered off.");
  }

  playMusic(): void {
    console.log("Playing music.");
  }
}
```
### **Why is this better?**
✅ **LightBulb** only implements `Switchable`.  
✅ **Speaker** implements both `Switchable` and `MusicPlayer`.  
✅ **New devices can be added without modifying unrelated code.**

---

## **How to Notice Bad ISP Implementation in Your Code**
1. **Are there empty or `throw new Error()` methods?**  
   - If a class is implementing a method just because the interface forces it to, it’s a red flag.  

2. **Do unrelated classes implement the same interface?**  
   - If two classes share an interface but perform **very different functions**, consider splitting it.  

3. **Does adding a new method to an interface affect many classes?**  
   - If yes, it means the interface is too broad, and changes ripple through unrelated parts of your system.  

---

## **Final Thoughts**
- **Bad ISP design = large, unfocused interfaces that force classes to implement unnecessary methods.**
- **Good ISP design = small, role-specific interfaces that keep implementations clean and modular.**
- **When in doubt, ask: "Does every class that implements this interface need all of its methods?" If not, split the interface.**
