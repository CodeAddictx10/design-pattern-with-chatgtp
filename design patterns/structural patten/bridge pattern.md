### **Bridge Design Pattern - Detailed Breakdown with Tons of Code Samples (TypeScript & PHP)**

---

## **What is the Bridge Design Pattern?**
The **Bridge Pattern** is a structural design pattern that separates abstraction from implementation so that the two can evolve independently. This helps in handling complexity and improving maintainability.

### **Key Concepts of the Bridge Pattern:**
1. **Abstraction**: The high-level control that delegates the work to an implementer.
2. **Implementation**: The actual logic that executes the request.
3. **Decoupling**: The abstraction and implementation are connected via composition, not inheritance.

---

# **1️⃣ Real-Life Scenario: Notification System**
Imagine we have a notification system where we can send messages via **Email** and **SMS**. Additionally, we may have different notification types, such as **Simple Notification** and **Urgent Notification**.

## **Step-by-Step Breakdown**
- **Abstraction**: `Notification` class handles high-level operations.
- **Refined Abstraction**: `SimpleNotification` and `UrgentNotification`.
- **Implementor**: `MessageSender` defines the message delivery method.
- **Concrete Implementors**: `EmailSender` and `SmsSender` implement actual sending logic.

---

## **✅ TypeScript Example**
```typescript
// Implementor
interface MessageSender {
    sendMessage(message: string): void;
}

// Concrete Implementors
class EmailSender implements MessageSender {
    sendMessage(message: string): void {
        console.log(`Sending email: ${message}`);
    }
}

class SmsSender implements MessageSender {
    sendMessage(message: string): void {
        console.log(`Sending SMS: ${message}`);
    }
}

// Abstraction
abstract class Notification {
    protected sender: MessageSender;

    constructor(sender: MessageSender) {
        this.sender = sender;
    }

    abstract notify(message: string): void;
}

// Refined Abstractions
class SimpleNotification extends Notification {
    notify(message: string): void {
        console.log("Simple Notification:");
        this.sender.sendMessage(message);
    }
}

class UrgentNotification extends Notification {
    notify(message: string): void {
        console.log("Urgent Notification:");
        this.sender.sendMessage(`URGENT: ${message}`);
    }
}

// Usage
const emailSender = new EmailSender();
const smsSender = new SmsSender();

const simpleEmailNotification = new SimpleNotification(emailSender);
const urgentSmsNotification = new UrgentNotification(smsSender);

simpleEmailNotification.notify("Meeting at 10 AM");
urgentSmsNotification.notify("Server is down!");
```

### **📝 Output:**
```
Simple Notification:
Sending email: Meeting at 10 AM
Urgent Notification:
Sending SMS: URGENT: Server is down!
```

---

## **✅ PHP Example**
```php
<?php
// Implementor
interface MessageSender {
    public function sendMessage(string $message): void;
}

// Concrete Implementors
class EmailSender implements MessageSender {
    public function sendMessage(string $message): void {
        echo "Sending email: $message\n";
    }
}

class SmsSender implements MessageSender {
    public function sendMessage(string $message): void {
        echo "Sending SMS: $message\n";
    }
}

// Abstraction
abstract class Notification {
    protected MessageSender $sender;

    public function __construct(MessageSender $sender) {
        $this->sender = $sender;
    }

    abstract public function notify(string $message): void;
}

// Refined Abstractions
class SimpleNotification extends Notification {
    public function notify(string $message): void {
        echo "Simple Notification:\n";
        $this->sender->sendMessage($message);
    }
}

class UrgentNotification extends Notification {
    public function notify(string $message): void {
        echo "Urgent Notification:\n";
        $this->sender->sendMessage("URGENT: $message");
    }
}

// Usage
$emailSender = new EmailSender();
$smsSender = new SmsSender();

$simpleEmailNotification = new SimpleNotification($emailSender);
$urgentSmsNotification = new UrgentNotification($smsSender);

$simpleEmailNotification->notify("Meeting at 10 AM");
$urgentSmsNotification->notify("Server is down!");
```

### **📝 Output:**
```
Simple Notification:
Sending email: Meeting at 10 AM
Urgent Notification:
Sending SMS: URGENT: Server is down!
```

---

# **2️⃣ Another Example: Remote Controls for Devices**
We have multiple **remote controls** (`BasicRemote`, `AdvancedRemote`) and multiple **devices** (`TV`, `Radio`). Instead of tightly coupling them, we use the **Bridge Pattern**.

## **✅ TypeScript Example**
```typescript
// Implementor
interface Device {
    turnOn(): void;
    turnOff(): void;
}

// Concrete Implementors
class TV implements Device {
    turnOn(): void {
        console.log("TV is now ON");
    }
    turnOff(): void {
        console.log("TV is now OFF");
    }
}

class Radio implements Device {
    turnOn(): void {
        console.log("Radio is now ON");
    }
    turnOff(): void {
        console.log("Radio is now OFF");
    }
}

// Abstraction
abstract class RemoteControl {
    protected device: Device;

    constructor(device: Device) {
        this.device = device;
    }

    abstract togglePower(): void;
}

// Refined Abstraction
class BasicRemote extends RemoteControl {
    togglePower(): void {
        console.log("Basic Remote:");
        this.device.turnOn();
    }
}

class AdvancedRemote extends RemoteControl {
    togglePower(): void {
        console.log("Advanced Remote:");
        this.device.turnOn();
        console.log("Setting volume to 50%");
    }
}

// Usage
const tv = new TV();
const radio = new Radio();

const basicTvRemote = new BasicRemote(tv);
const advancedRadioRemote = new AdvancedRemote(radio);

basicTvRemote.togglePower();
advancedRadioRemote.togglePower();
```

---

## **🛑 Signs of a Bad Implementation**
1. **Tightly Coupled Code**: If `Notification` directly depends on `EmailSender` or `SmsSender`, adding new types requires modifying existing code.
2. **Too Many Concrete Classes**: If every new notification type has a subclass for each sender type, it's a sign that Bridge Pattern could be applied.
3. **Inheritance Instead of Composition**: If `NotificationEmail` extends `Notification` and `NotificationSms` extends `Notification`, then abstraction and implementation are not separated.

---

# **✅ Summary**
- **Bridge Pattern** separates abstraction from implementation, making code flexible and extendable.
- **Example 1**: Notification system (`SimpleNotification`, `UrgentNotification` with `EmailSender`, `SmsSender`).
- **Example 2**: Remote controls (`BasicRemote`, `AdvancedRemote` with `TV`, `Radio`).
- **Bad Implementations**: Overusing inheritance, too many subclasses, tightly coupled code.

By applying the **Bridge Pattern**, you avoid deep inheritance trees and promote modularity. 🚀


### **🚨 Bad Implementation: When the Bridge Pattern Should Be Used but Isn't**  

Let's take the **notification system** example, but implement it **without** the Bridge Pattern. This will demonstrate why a **bad design** makes code inflexible and difficult to maintain.

---

## **🚫 Bad Implementation (Tightly Coupled)**
In this design, **each notification type (`SimpleNotification`, `UrgentNotification`) is directly tied to a specific message sender (`EmailNotification`, `SmsNotification`)**.  

This results in:
- **Code Duplication**: Each notification type must have a separate class for email and SMS.
- **Lack of Scalability**: Adding a new notification type (e.g., `ScheduledNotification`) requires duplicating the logic for both `Email` and `SMS`.
- **Violation of Open/Closed Principle**: Every time a new sender is added (e.g., `PushNotification`), we need to modify multiple classes.

---

### **🚫 TypeScript Bad Implementation**
```typescript
// Each notification type is tied to a specific message sender

class EmailSimpleNotification {
    notify(message: string): void {
        console.log(`Sending Simple Email: ${message}`);
    }
}

class SmsSimpleNotification {
    notify(message: string): void {
        console.log(`Sending Simple SMS: ${message}`);
    }
}

class EmailUrgentNotification {
    notify(message: string): void {
        console.log(`Sending Urgent Email: URGENT! ${message}`);
    }
}

class SmsUrgentNotification {
    notify(message: string): void {
        console.log(`Sending Urgent SMS: URGENT! ${message}`);
    }
}

// Usage
const emailSimple = new EmailSimpleNotification();
emailSimple.notify("Meeting at 10 AM");

const smsUrgent = new SmsUrgentNotification();
smsUrgent.notify("Server is down!");
```

### **📝 Output:**
```
Sending Simple Email: Meeting at 10 AM
Sending Urgent SMS: URGENT! Server is down!
```

---

### **🚫 PHP Bad Implementation**
```php
<?php
// Each notification type is tied to a specific message sender

class EmailSimpleNotification {
    public function notify(string $message): void {
        echo "Sending Simple Email: $message\n";
    }
}

class SmsSimpleNotification {
    public function notify(string $message): void {
        echo "Sending Simple SMS: $message\n";
    }
}

class EmailUrgentNotification {
    public function notify(string $message): void {
        echo "Sending Urgent Email: URGENT! $message\n";
    }
}

class SmsUrgentNotification {
    public function notify(string $message): void {
        echo "Sending Urgent SMS: URGENT! $message\n";
    }
}

// Usage
$emailSimple = new EmailSimpleNotification();
$emailSimple->notify("Meeting at 10 AM");

$smsUrgent = new SmsUrgentNotification();
$smsUrgent->notify("Server is down!");
```

### **📝 Output:**
```
Sending Simple Email: Meeting at 10 AM
Sending Urgent SMS: URGENT! Server is down!
```

---

## **🛑 Problems in the Above Code**
1. **Class Explosion**: If we add a new notification type (`ScheduledNotification`), we must create:
   - `EmailScheduledNotification`
   - `SmsScheduledNotification`
   - If a new sender like `PushNotification` is introduced, it further multiplies the classes.
   
2. **Tightly Coupled Code**: Each notification is **directly tied** to an implementation (`Email` or `SMS`). There's no flexibility to swap implementations dynamically.

3. **Hard to Extend**: If we need to introduce a **Push Notification**, we'd have to duplicate logic across multiple classes.

---

## **✅ Refactoring Using the Bridge Pattern**
Instead of creating multiple classes for each combination, **decouple the notification type from the message sender** by using the **Bridge Pattern**.

✅ **Fix:**  
- **Abstract Notification Class**
- **MessageSender Interface**
- **Concrete Implementations of MessageSender (EmailSender, SmsSender)**

Here’s the **correct implementation** using the **Bridge Pattern**:

### **TypeScript - Fixed Code**
```typescript
// Implementor
interface MessageSender {
    sendMessage(message: string): void;
}

// Concrete Implementors
class EmailSender implements MessageSender {
    sendMessage(message: string): void {
        console.log(`Sending email: ${message}`);
    }
}

class SmsSender implements MessageSender {
    sendMessage(message: string): void {
        console.log(`Sending SMS: ${message}`);
    }
}

// Abstraction
abstract class Notification {
    protected sender: MessageSender;

    constructor(sender: MessageSender) {
        this.sender = sender;
    }

    abstract notify(message: string): void;
}

// Refined Abstractions
class SimpleNotification extends Notification {
    notify(message: string): void {
        console.log("Simple Notification:");
        this.sender.sendMessage(message);
    }
}

class UrgentNotification extends Notification {
    notify(message: string): void {
        console.log("Urgent Notification:");
        this.sender.sendMessage(`URGENT: ${message}`);
    }
}

// Usage
const emailSender = new EmailSender();
const smsSender = new SmsSender();

const simpleEmailNotification = new SimpleNotification(emailSender);
const urgentSmsNotification = new UrgentNotification(smsSender);

simpleEmailNotification.notify("Meeting at 10 AM");
urgentSmsNotification.notify("Server is down!");
```

---

### **PHP - Fixed Code**
```php
<?php
// Implementor
interface MessageSender {
    public function sendMessage(string $message): void;
}

// Concrete Implementors
class EmailSender implements MessageSender {
    public function sendMessage(string $message): void {
        echo "Sending email: $message\n";
    }
}

class SmsSender implements MessageSender {
    public function sendMessage(string $message): void {
        echo "Sending SMS: $message\n";
    }
}

// Abstraction
abstract class Notification {
    protected MessageSender $sender;

    public function __construct(MessageSender $sender) {
        $this->sender = $sender;
    }

    abstract public function notify(string $message): void;
}

// Refined Abstractions
class SimpleNotification extends Notification {
    public function notify(string $message): void {
        echo "Simple Notification:\n";
        $this->sender->sendMessage($message);
    }
}

class UrgentNotification extends Notification {
    public function notify(string $message): void {
        echo "Urgent Notification:\n";
        $this->sender->sendMessage("URGENT: $message");
    }
}

// Usage
$emailSender = new EmailSender();
$smsSender = new SmsSender();

$simpleEmailNotification = new SimpleNotification($emailSender);
$urgentSmsNotification = new UrgentNotification($smsSender);

$simpleEmailNotification->notify("Meeting at 10 AM");
$urgentSmsNotification->notify("Server is down!");
```

---

## **🎯 Key Takeaways**
| **Criteria**            | **Bad Implementation** 😡 | **Bridge Pattern Implementation** 😍 |
|-------------------------|---------------------------|----------------------------------|
| **Flexibility**        | Hardcoded sender logic    | Easily swap senders dynamically |
| **Scalability**        | Class explosion           | Minimal new classes needed      |
| **Code Duplication**   | High                      | Low                             |
| **Extensibility**      | Requires modifying existing code | Just add a new implementation |

---

## **✅ Summary**
- **Bad Implementation**: Tightly couples notification types and senders, leading to class explosion.
- **Good Implementation (Bridge Pattern)**: Separates notification types and senders using composition, making the system **more flexible and scalable**.

🔥 **By using the Bridge Pattern, we reduce duplication, improve maintainability, and make the system open for extension but closed for modification!** 🚀
