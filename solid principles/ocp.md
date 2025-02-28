The **Open/Closed Principle (OCP)** states that:  
*A class should be open for extension but closed for modification.*

This means you should be able to add new functionality without modifying existing code. Instead of changing a class when requirements change, you should extend it.

---

### 🚗 Real-Life Example: **Vehicle Insurance Premium Calculation**  

Imagine an insurance company needs to calculate premiums for different types of vehicles (cars, bikes, and trucks).  
We'll first write a **bad example (violating OCP)** and then refactor it using **OCP properly**.

---

## ❌ **Bad Example (Violates OCP)**
Here, we modify the `InsuranceCalculator` every time a new vehicle type is added. This violates OCP.

### TypeScript
```typescript
class InsuranceCalculator {
  calculatePremium(vehicleType: string, basePrice: number): number {
    if (vehicleType === "car") {
      return basePrice * 1.1;
    } else if (vehicleType === "bike") {
      return basePrice * 1.05;
    } else if (vehicleType === "truck") {
      return basePrice * 1.2;
    }
    throw new Error("Unknown vehicle type");
  }
}

const calculator = new InsuranceCalculator();
console.log(calculator.calculatePremium("car", 1000));
console.log(calculator.calculatePremium("bike", 1000));
console.log(calculator.calculatePremium("truck", 1000));
```

---

### PHP
```php
class InsuranceCalculator {
    public function calculatePremium(string $vehicleType, float $basePrice): float {
        if ($vehicleType === "car") {
            return $basePrice * 1.1;
        } elseif ($vehicleType === "bike") {
            return $basePrice * 1.05;
        } elseif ($vehicleType === "truck") {
            return $basePrice * 1.2;
        }
        throw new Exception("Unknown vehicle type");
    }
}

$calculator = new InsuranceCalculator();
echo $calculator->calculatePremium("car", 1000) . PHP_EOL;
echo $calculator->calculatePremium("bike", 1000) . PHP_EOL;
echo $calculator->calculatePremium("truck", 1000) . PHP_EOL;
```

### ❌ **Why is this bad?**
- Every time a new vehicle type is added (e.g., "bus" or "scooter"), we must **modify the class**.
- It **violates OCP** because the class is not closed for modification.

---

## ✅ **Good Example (Follows OCP)**
Instead of modifying the class, we use **polymorphism** to create different classes for each vehicle type.

### TypeScript (Using Interface)
```typescript
interface Vehicle {
  calculatePremium(basePrice: number): number;
}

class Car implements Vehicle {
  calculatePremium(basePrice: number): number {
    return basePrice * 1.1;
  }
}

class Bike implements Vehicle {
  calculatePremium(basePrice: number): number {
    return basePrice * 1.05;
  }
}

class Truck implements Vehicle {
  calculatePremium(basePrice: number): number {
    return basePrice * 1.2;
  }
}

// New vehicle types can be added without modifying existing code
class Bus implements Vehicle {
  calculatePremium(basePrice: number): number {
    return basePrice * 1.3;
  }
}

// Usage
const vehicles: Vehicle[] = [new Car(), new Bike(), new Truck(), new Bus()];

vehicles.forEach((vehicle) => {
  console.log(vehicle.calculatePremium(1000));
});
```

---

### PHP (Using Interface)
```php
interface Vehicle {
    public function calculatePremium(float $basePrice): float;
}

class Car implements Vehicle {
    public function calculatePremium(float $basePrice): float {
        return $basePrice * 1.1;
    }
}

class Bike implements Vehicle {
    public function calculatePremium(float $basePrice): float {
        return $basePrice * 1.05;
    }
}

class Truck implements Vehicle {
    public function calculatePremium(float $basePrice): float {
        return $basePrice * 1.2;
    }
}

// New vehicle types can be added without modifying existing code
class Bus implements Vehicle {
    public function calculatePremium(float $basePrice): float {
        return $basePrice * 1.3;
    }
}

// Usage
$vehicles = [new Car(), new Bike(), new Truck(), new Bus()];

foreach ($vehicles as $vehicle) {
    echo $vehicle->calculatePremium(1000) . PHP_EOL;
}
```

---

### ✅ **Why is this better?**
- The `InsuranceCalculator` class **does not need modification** when a new vehicle type is added.
- It **follows OCP** by allowing extensions without altering existing code.
- It makes the system **more scalable and maintainable**.

---

## 🚀 Another Example: **Payment Processing System**
Imagine an e-commerce app where different payment methods (Credit Card, PayPal, Bitcoin) are available.

---

### ❌ **Bad Example (Violates OCP)**
```typescript
class PaymentProcessor {
  processPayment(method: string, amount: number) {
    if (method === "creditCard") {
      console.log(`Processing credit card payment of $${amount}`);
    } else if (method === "paypal") {
      console.log(`Processing PayPal payment of $${amount}`);
    } else if (method === "bitcoin") {
      console.log(`Processing Bitcoin payment of $${amount}`);
    } else {
      throw new Error("Unknown payment method");
    }
  }
}
```

```php
class PaymentProcessor {
    public function processPayment(string $method, float $amount) {
        if ($method === "creditCard") {
            echo "Processing credit card payment of $$amount" . PHP_EOL;
        } elseif ($method === "paypal") {
            echo "Processing PayPal payment of $$amount" . PHP_EOL;
        } elseif ($method === "bitcoin") {
            echo "Processing Bitcoin payment of $$amount" . PHP_EOL;
        } else {
            throw new Exception("Unknown payment method");
        }
    }
}
```

🔴 **Issue:** We have to edit this class **every time** a new payment method is introduced.

---

### ✅ **Good Example (Follows OCP)**
```typescript
interface PaymentMethod {
  process(amount: number): void;
}

class CreditCard implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing credit card payment of $${amount}`);
  }
}

class PayPal implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing PayPal payment of $${amount}`);
  }
}

class Bitcoin implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing Bitcoin payment of $${amount}`);
  }
}

// Adding a new payment method without modifying existing code
class ApplePay implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing Apple Pay payment of $${amount}`);
  }
}

// Usage
const payments: PaymentMethod[] = [new CreditCard(), new PayPal(), new Bitcoin(), new ApplePay()];
payments.forEach(payment => payment.process(100));
```

---

```php
interface PaymentMethod {
    public function process(float $amount): void;
}

class CreditCard implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing credit card payment of $$amount" . PHP_EOL;
    }
}

class PayPal implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing PayPal payment of $$amount" . PHP_EOL;
    }
}

class Bitcoin implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing Bitcoin payment of $$amount" . PHP_EOL;
    }
}

// Adding a new payment method without modifying existing code
class ApplePay implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing Apple Pay payment of $$amount" . PHP_EOL;
    }
}

// Usage
$payments = [new CreditCard(), new PayPal(), new Bitcoin(), new ApplePay()];
foreach ($payments as $payment) {
    $payment->process(100);
}
```

---

### ✅ **Why is this better?**
- No modification is needed in the existing `PaymentProcessor` class.
- New payment methods can be added by **creating new classes**.
- The system is **scalable** and **maintainable**.

---

### **Key Takeaways**
✔ Keep the core class **closed for modification**.  
✔ Use **abstraction (interfaces or base classes)** to allow **extension**.  
✔ Avoid **modifying existing code** to prevent bugs and maintain stability.  


Great question! Spotting a **bad implementation of the Open/Closed Principle (OCP)** requires recognizing **code smells** that indicate your code **isn't open for extension and closed for modification**. Here are key signs to look out for:  

---

## 🚨 **1. If-Else or Switch Statements Based on Type**
### 🔴 **Bad Sign:**  
If you see **if-else** or **switch statements** that check for different types or categories, it's a sign that your code will need to be modified every time a new type is added.

### ❌ **Example: Insurance Calculator (Violating OCP)**
```typescript
class InsuranceCalculator {
  calculatePremium(vehicleType: string, basePrice: number): number {
    if (vehicleType === "car") {
      return basePrice * 1.1;
    } else if (vehicleType === "bike") {
      return basePrice * 1.05;
    } else if (vehicleType === "truck") {
      return basePrice * 1.2;
    }
    throw new Error("Unknown vehicle type");
  }
}
```

```php
class InsuranceCalculator {
    public function calculatePremium(string $vehicleType, float $basePrice): float {
        if ($vehicleType === "car") {
            return $basePrice * 1.1;
        } elseif ($vehicleType === "bike") {
            return $basePrice * 1.05;
        } elseif ($vehicleType === "truck") {
            return $basePrice * 1.2;
        }
        throw new Exception("Unknown vehicle type");
    }
}
```
🛑 **Why is this bad?**  
- Every time a new vehicle type is added, we must **modify** this method.  
- The `calculatePremium` function should **not be modified**, but rather **extended**.

✅ **Solution:** Use polymorphism and create separate classes for each vehicle type.  

---

## 🚨 **2. Constantly Changing Core Classes**
### 🔴 **Bad Sign:**  
If you're frequently **modifying existing classes** when adding new features, it's a sign that your code is not **closed for modification**.

### ❌ **Example: Payment Processing**
```typescript
class PaymentProcessor {
  processPayment(method: string, amount: number) {
    if (method === "creditCard") {
      console.log(`Processing credit card payment of $${amount}`);
    } else if (method === "paypal") {
      console.log(`Processing PayPal payment of $${amount}`);
    } else if (method === "bitcoin") {
      console.log(`Processing Bitcoin payment of $${amount}`);
    } else {
      throw new Error("Unknown payment method");
    }
  }
}
```

```php
class PaymentProcessor {
    public function processPayment(string $method, float $amount) {
        if ($method === "creditCard") {
            echo "Processing credit card payment of $$amount" . PHP_EOL;
        } elseif ($method === "paypal") {
            echo "Processing PayPal payment of $$amount" . PHP_EOL;
        } elseif ($method === "bitcoin") {
            echo "Processing Bitcoin payment of $$amount" . PHP_EOL;
        } else {
            throw new Exception("Unknown payment method");
        }
    }
}
```
🛑 **Why is this bad?**  
- If we introduce **Apple Pay**, we must modify the class again.  
- Every new payment method requires changes to this function, making it **prone to errors** and **hard to maintain**.

✅ **Solution:**  
Create an interface and extend functionality using separate classes.  

---

## 🚨 **3. Violation of Single Responsibility Principle (SRP)**
### 🔴 **Bad Sign:**  
If a class is **doing too much** (e.g., handling multiple responsibilities), it's often an indication that it **doesn't follow OCP** properly.  

### ❌ **Example: Order Processor Doing Everything**
```typescript
class OrderProcessor {
  processOrder(orderId: number) {
    console.log(`Processing order ${orderId}`);
  }

  sendEmailNotification(orderId: number) {
    console.log(`Sending email for order ${orderId}`);
  }

  generateInvoice(orderId: number) {
    console.log(`Generating invoice for order ${orderId}`);
  }
}
```

```php
class OrderProcessor {
    public function processOrder(int $orderId) {
        echo "Processing order $orderId" . PHP_EOL;
    }

    public function sendEmailNotification(int $orderId) {
        echo "Sending email for order $orderId" . PHP_EOL;
    }

    public function generateInvoice(int $orderId) {
        echo "Generating invoice for order $orderId" . PHP_EOL;
    }
}
```
🛑 **Why is this bad?**  
- If we need to add **SMS notifications**, we must modify this class.  
- This class **handles multiple responsibilities**, which means it's **not easily extendable**.  

✅ **Solution:**  
- Separate responsibilities into different **interfaces or classes**.  

---

## 🚨 **4. Hard-Coded Dependencies**
### 🔴 **Bad Sign:**  
If a class **directly instantiates objects of other classes**, it **violates OCP** because adding new implementations **requires modifying existing code**.

### ❌ **Example: Report Generator**
```typescript
class ReportGenerator {
  generate(reportType: string) {
    if (reportType === "pdf") {
      console.log("Generating PDF Report...");
    } else if (reportType === "csv") {
      console.log("Generating CSV Report...");
    } else {
      throw new Error("Unknown report type");
    }
  }
}
```

```php
class ReportGenerator {
    public function generate(string $reportType) {
        if ($reportType === "pdf") {
            echo "Generating PDF Report..." . PHP_EOL;
        } elseif ($reportType === "csv") {
            echo "Generating CSV Report..." . PHP_EOL;
        } else {
            throw new Exception("Unknown report type");
        }
    }
}
```
🛑 **Why is this bad?**  
- Adding a new report type (e.g., **Excel**) requires modifying this class.  
- This violates **both OCP and SRP**.  

✅ **Solution:**  
Use an interface for different report types.

---

## ✅ **How to Fix These Issues?**
### 🔥 **Use Abstraction & Polymorphism**
Instead of using **if-else**, create an **interface** and let individual classes handle specific logic.

### 🔥 **Example: Fixing Payment Processor with OCP**
#### **TypeScript**
```typescript
interface PaymentMethod {
  process(amount: number): void;
}

class CreditCard implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing credit card payment of $${amount}`);
  }
}

class PayPal implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing PayPal payment of $${amount}`);
  }
}

// New payment methods can be added without modifying existing code
class Bitcoin implements PaymentMethod {
  process(amount: number): void {
    console.log(`Processing Bitcoin payment of $${amount}`);
  }
}

// Usage
const payments: PaymentMethod[] = [new CreditCard(), new PayPal(), new Bitcoin()];
payments.forEach(payment => payment.process(100));
```

#### **PHP**
```php
interface PaymentMethod {
    public function process(float $amount): void;
}

class CreditCard implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing credit card payment of $$amount" . PHP_EOL;
    }
}

class PayPal implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing PayPal payment of $$amount" . PHP_EOL;
    }
}

// New payment methods can be added without modifying existing code
class Bitcoin implements PaymentMethod {
    public function process(float $amount): void {
        echo "Processing Bitcoin payment of $$amount" . PHP_EOL;
    }
}

// Usage
$payments = [new CreditCard(), new PayPal(), new Bitcoin()];
foreach ($payments as $payment) {
    $payment->process(100);
}
```

---

## 🔥 **Final Takeaways**
✔ **Look for `if-else` or `switch` statements checking types**  
✔ **Check if existing classes change frequently**  
✔ **Ensure classes only have one responsibility (SRP)**  
✔ **Use abstraction (interfaces or base classes) to extend functionality**  