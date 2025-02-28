I'll break down the **Factory Method Design Pattern** step by step, using **real-life examples** in **TypeScript** and **PHP** side by side. I'll also show **bad implementations** and how to **identify mistakes** when using this pattern.

---

## **1. What is the Factory Method Pattern?**
The **Factory Method** is a **creational design pattern** that provides an interface for creating objects in a superclass but **lets subclasses alter the type of objects that will be created**.

### **Real-Life Analogy**
Imagine a **food delivery app** like Uber Eats. You can order different types of food (**Pizza, Sushi, or Burger**) from various restaurants. Each restaurant follows its own recipe but delivers the same interface (`Food`).

Instead of instantiating `Pizza` or `Sushi` directly, we let a **factory** decide which food to create.

---

# **2. Factory Method: TypeScript vs PHP**
## **Good Example: Food Delivery App**

### **Step 1: Define a Common Interface**
Every food item should have a **common interface**.

#### **TypeScript**
```ts
interface Food {
  prepare(): void;
  deliver(): void;
}
```

#### **PHP**
```php
interface Food {
    public function prepare(): void;
    public function deliver(): void;
}
```

---

### **Step 2: Create Concrete Classes**
Each food item implements the `Food` interface.

#### **TypeScript**
```ts
class Pizza implements Food {
  prepare(): void {
    console.log("Preparing Pizza...");
  }
  deliver(): void {
    console.log("Delivering Pizza...");
  }
}

class Sushi implements Food {
  prepare(): void {
    console.log("Preparing Sushi...");
  }
  deliver(): void {
    console.log("Delivering Sushi...");
  }
}
```

#### **PHP**
```php
class Pizza implements Food {
    public function prepare(): void {
        echo "Preparing Pizza...\n";
    }
    public function deliver(): void {
        echo "Delivering Pizza...\n";
    }
}

class Sushi implements Food {
    public function prepare(): void {
        echo "Preparing Sushi...\n";
    }
    public function deliver(): void {
        echo "Delivering Sushi...\n";
    }
}
```

---

### **Step 3: Create the Factory Method**
Each restaurant has a **method to create food**, but **subclasses decide which food to prepare**.

#### **TypeScript**
```ts
abstract class Restaurant {
  abstract createFood(): Food;

  orderFood(): Food {
    const food = this.createFood();
    food.prepare();
    food.deliver();
    return food;
  }
}

class PizzaRestaurant extends Restaurant {
  createFood(): Food {
    return new Pizza();
  }
}

class SushiRestaurant extends Restaurant {
  createFood(): Food {
    return new Sushi();
  }
}
```

#### **PHP**
```php
abstract class Restaurant {
    abstract public function createFood(): Food;

    public function orderFood(): Food {
        $food = $this->createFood();
        $food->prepare();
        $food->deliver();
        return $food;
    }
}

class PizzaRestaurant extends Restaurant {
    public function createFood(): Food {
        return new Pizza();
    }
}

class SushiRestaurant extends Restaurant {
    public function createFood(): Food {
        return new Sushi();
    }
}
```

---

### **Step 4: Use the Factory**
The client code **does not directly instantiate `Pizza` or `Sushi`**, instead, it interacts with the `Restaurant` class.

#### **TypeScript**
```ts
const pizzaPlace = new PizzaRestaurant();
pizzaPlace.orderFood();

const sushiPlace = new SushiRestaurant();
sushiPlace.orderFood();
```

#### **PHP**
```php
$pizzaPlace = new PizzaRestaurant();
$pizzaPlace->orderFood();

$sushiPlace = new SushiRestaurant();
$sushiPlace->orderFood();
```

### **Output**
```
Preparing Pizza...
Delivering Pizza...
Preparing Sushi...
Delivering Sushi...
```
---

## **3. BAD Implementation of Factory Method (How to Spot Issues)**

### **❌ Mistake 1: Violating Open/Closed Principle**
A **bad** factory method directly modifies the factory to add new types instead of using subclasses.

#### **Bad TypeScript**
```ts
class FoodFactory {
  static createFood(type: string): Food {
    if (type === "pizza") return new Pizza();
    if (type === "sushi") return new Sushi();
    throw new Error("Unknown food type");
  }
}
```

#### **Bad PHP**
```php
class FoodFactory {
    public static function createFood(string $type): Food {
        if ($type === "pizza") return new Pizza();
        if ($type === "sushi") return new Sushi();
        throw new Exception("Unknown food type");
    }
}
```
**❌ Why is this bad?**
- Every time we add a new food (`Burger`, `Pasta`), we need to **modify the factory**, breaking the **Open/Closed Principle**.
- This makes maintenance **harder**.

✅ **Solution:** Use **subclasses** instead of conditionals.

---

### **❌ Mistake 2: Factory Method Returning Wrong Objects**
If the factory method **returns objects that do not implement the expected interface**, it can cause runtime errors.

#### **Bad TypeScript**
```ts
class PizzaRestaurant extends Restaurant {
  createFood(): any {
    return "This is not a food object";
  }
}
```
#### **Bad PHP**
```php
class PizzaRestaurant extends Restaurant {
    public function createFood() {
        return "This is not a food object";
    }
}
```
**❌ Why is this bad?**
- The return type should be `Food`, but instead, we are returning a `string`, causing **runtime errors**.

✅ **Solution:** Always return an instance of the expected interface.

---

### **❌ Mistake 3: Direct Instantiation Instead of Factory**
A factory should be responsible for object creation. If a class instantiates objects directly, it defeats the purpose.

#### **Bad TypeScript**
```ts
class Order {
  private food: Food;

  constructor() {
    this.food = new Pizza(); // ❌ Should use factory
  }
}
```
#### **Bad PHP**
```php
class Order {
    private Food $food;

    public function __construct() {
        $this->food = new Pizza(); // ❌ Should use factory
    }
}
```
**❌ Why is this bad?**
- This hardcodes `Pizza`, making it **impossible** to switch to `Sushi` without modifying this class.
- **Factory should decide** which food to create.

✅ **Solution:** Use the **factory method** to create objects dynamically.

---

## **4. Summary**
### **✅ Good Factory Method**
✔ Uses an **abstract creator class** (`Restaurant`).  
✔ Subclasses **override** the `createFood()` method.  
✔ New types can be added **without modifying existing code**.  

### **❌ Bad Factory Method**
❌ Uses **if-else or switch** in the factory method.  
❌ Returns an object that does not implement the expected interface.  
❌ Directly instantiates objects instead of using a factory.  

---

## **Final Thoughts**
The **Factory Method** helps when you **want to delegate object creation to subclasses**. It prevents **tight coupling** and allows **extending functionality without modifying existing code**.
