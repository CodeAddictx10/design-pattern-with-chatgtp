### **Understanding the Liskov Substitution Principle (LSP)**  
The **Liskov Substitution Principle (LSP)** states that **a subclass should be substitutable for its superclass without changing the correctness of the program**.  

If you have a function that depends on a **parent class**, it should be able to accept **any child class** without breaking its behavior.  

---

### **Real-Life Example: Payment Processing System**
Imagine we have a **PaymentProcessor** class for handling online payments. It has methods for processing payments, refunding transactions, and getting the transaction status.

We'll implement **two different payment methods (PayPal and Bitcoin)** while ensuring that our subclasses follow LSP.

---

## ✅ **Good Implementation (Following LSP)**
Each subclass correctly extends the parent class without breaking functionality.

### **TypeScript (Following LSP)**
```ts
// Base class: Payment Processor
abstract class PaymentProcessor {
  constructor(protected amount: number) {}

  abstract processPayment(): string;
  abstract refundPayment(): string;
  abstract getTransactionStatus(): string;
}

// PayPal Processor (inherits correctly)
class PayPalProcessor extends PaymentProcessor {
  processPayment(): string {
    return `Processing PayPal payment of $${this.amount}`;
  }

  refundPayment(): string {
    return `Refunding PayPal payment of $${this.amount}`;
  }

  getTransactionStatus(): string {
    return `PayPal transaction status: SUCCESS`;
  }
}

// Bitcoin Processor (inherits correctly)
class BitcoinProcessor extends PaymentProcessor {
  processPayment(): string {
    return `Processing Bitcoin payment of $${this.amount}`;
  }

  refundPayment(): string {
    return `Refunding Bitcoin payment of $${this.amount}`;
  }

  getTransactionStatus(): string {
    return `Bitcoin transaction status: SUCCESS`;
  }
}

// Function that works with any PaymentProcessor
function processOrder(payment: PaymentProcessor) {
  console.log(payment.processPayment());
  console.log(payment.getTransactionStatus());
}

const paypal = new PayPalProcessor(100);
const bitcoin = new BitcoinProcessor(200);

processOrder(paypal);
processOrder(bitcoin);
```

---

### **PHP (Following LSP)**
```php
<?php

// Base class: PaymentProcessor
abstract class PaymentProcessor {
    protected float $amount;

    public function __construct(float $amount) {
        $this->amount = $amount;
    }

    abstract public function processPayment(): string;
    abstract public function refundPayment(): string;
    abstract public function getTransactionStatus(): string;
}

// PayPal Processor (inherits correctly)
class PayPalProcessor extends PaymentProcessor {
    public function processPayment(): string {
        return "Processing PayPal payment of \${$this->amount}";
    }

    public function refundPayment(): string {
        return "Refunding PayPal payment of \${$this->amount}";
    }

    public function getTransactionStatus(): string {
        return "PayPal transaction status: SUCCESS";
    }
}

// Bitcoin Processor (inherits correctly)
class BitcoinProcessor extends PaymentProcessor {
    public function processPayment(): string {
        return "Processing Bitcoin payment of \${$this->amount}";
    }

    public function refundPayment(): string {
        return "Refunding Bitcoin payment of \${$this->amount}";
    }

    public function getTransactionStatus(): string {
        return "Bitcoin transaction status: SUCCESS";
    }
}

// Function that works with any PaymentProcessor
function processOrder(PaymentProcessor $payment) {
    echo $payment->processPayment() . PHP_EOL;
    echo $payment->getTransactionStatus() . PHP_EOL;
}

$paypal = new PayPalProcessor(100);
$bitcoin = new BitcoinProcessor(200);

processOrder($paypal);
processOrder($bitcoin);
```

🔹 **Why is this a Good Implementation?**
- Each subclass implements **all the methods** from the parent class.
- `processOrder()` function does not need to check the type of payment—it works for both PayPal and Bitcoin.
- The subclasses do not **remove behavior** from the parent class.

---

## ❌ **Bad Implementation (Breaking LSP)**
What happens if one subclass **removes or changes the behavior** of the parent class in a way that breaks expectations?

### **TypeScript (Breaking LSP)**
```ts
// A new class that breaks LSP
class CashOnDeliveryProcessor extends PaymentProcessor {
  processPayment(): string {
    return `Cash on Delivery selected. Payment is not processed online.`;
  }

  // ❌ Violates LSP: Refunds are not supported, but method still exists
  refundPayment(): string {
    throw new Error("Refunds are not supported for Cash on Delivery.");
  }

  getTransactionStatus(): string {
    return `Cash on Delivery transaction status: PENDING`;
  }
}

const cod = new CashOnDeliveryProcessor(50);
console.log(cod.processPayment()); // Works fine
console.log(cod.refundPayment());  // ❌ Throws an error (unexpected behavior)
```

---

### **PHP (Breaking LSP)**
```php
<?php

class CashOnDeliveryProcessor extends PaymentProcessor {
    public function processPayment(): string {
        return "Cash on Delivery selected. Payment is not processed online.";
    }

    // ❌ Violates LSP: Refunds should be supported, but it's not
    public function refundPayment(): string {
        throw new Exception("Refunds are not supported for Cash on Delivery.");
    }

    public function getTransactionStatus(): string {
        return "Cash on Delivery transaction status: PENDING";
    }
}

$cod = new CashOnDeliveryProcessor(50);
echo $cod->processPayment() . PHP_EOL; // Works fine
echo $cod->refundPayment();  // ❌ Throws an error (unexpected behavior)
```

🔹 **Why is this a Bad Implementation?**
- **CashOnDeliveryProcessor** breaks LSP because it removes refund functionality, making it incompatible with the parent class.
- If the `processOrder()` function tries to refund a `CashOnDeliveryProcessor`, it will **crash** unexpectedly.

---

## **How to Notice a Bad Implementation of OCP?**
The **Open/Closed Principle (OCP)** states that **classes should be open for extension but closed for modification**.

Here’s how to **detect bad implementations**:

1. **If you need to modify an existing class to add new behavior**  
   - ❌ **Bad**: You modify the `PaymentProcessor` class every time a new payment method is introduced.
   - ✅ **Good**: You create a new class (`BitcoinProcessor`, `PayPalProcessor`, etc.) without modifying the `PaymentProcessor` base class.

2. **If you use excessive conditionals (`if` or `switch`) instead of polymorphism**  
   - ❌ **Bad**:  
     ```ts
     function processOrder(payment) {
         if (payment.type === "PayPal") { ... }
         else if (payment.type === "Bitcoin") { ... }
         else { throw new Error("Unknown payment type"); }
     }
     ```
   - ✅ **Good**:  
     ```ts
     function processOrder(payment: PaymentProcessor) {
         console.log(payment.processPayment());
     }
     ```

3. **If a subclass overrides a method to throw an error instead of implementing it**  
   - ❌ **Bad**: `CashOnDeliveryProcessor` removes the `refundPayment()` method by throwing an error.
   - ✅ **Good**: All subclasses should **fully implement** the base class methods.

---

## **Conclusion**
🔹 **Liskov Substitution Principle (LSP)** ensures that **subtypes (child classes) can replace their base type (parent class) without breaking functionality**.  
🔹 **A bad implementation** usually occurs when a subclass:
   - **Removes** an expected method.
   - **Modifies** behavior in a way that breaks the parent class’s contract.
   - **Throws unexpected errors** in a method that should work for all subclasses.  
🔹 **Open/Closed Principle (OCP) violations** can be spotted when:
   - You **modify existing code** to add new features.
   - You rely on **if-else chains instead of polymorphism**.  

---

### **Do you need more examples?** Let me know what part still seems confusing! 🚀