The **Builder Design Pattern** is used to construct complex objects step by step. It allows you to create different representations of an object using the same construction code.

---

## 🔥 **Breaking It Down in Simple Steps**  
1. **Why Use the Builder Pattern?**  
   - When an object has too many parameters.
   - When an object's construction is complex (nested objects, optional parameters, etc.).
   - To improve readability and maintainability.
   - To create immutable objects step by step.

2. **Key Components of the Builder Pattern**
   - **Product**: The complex object being built.
   - **Builder Interface**: Defines the steps for building the product.
   - **Concrete Builder**: Implements the builder interface.
   - **Director (Optional)**: Controls the building process.

---

## 🏗️ **Basic Implementation (Step by Step)**

### **1️⃣ Without Builder (Bad Approach - Telescoping Constructor)**  
Imagine we are building a **Burger** object with multiple options.

#### **❌ Bad TypeScript Example (Too Many Parameters)**
```typescript
class Burger {
  constructor(
    public size: string,
    public cheese: boolean,
    public pepperoni: boolean,
    public lettuce: boolean
  ) {}
}

const burger = new Burger("Large", true, false, true);
```
#### **❌ Bad PHP Example (Too Many Parameters)**
```php
class Burger {
    public function __construct(
        public string $size,
        public bool $cheese,
        public bool $pepperoni,
        public bool $lettuce
    ) {}
}

$burger = new Burger("Large", true, false, true);
```
> **Problems**:  
> - Too many parameters.  
> - Hard to read.  
> - What does `true, false, true` even mean?  
> - If new ingredients are added, it breaks existing code.

---

### **2️⃣ Implementing the Builder Pattern (Good Approach)**  

### **✅ TypeScript Implementation**
```typescript
// Product: The complex object we want to build
class Burger {
  public size: string;
  public cheese: boolean = false;
  public pepperoni: boolean = false;
  public lettuce: boolean = false;

  constructor(size: string) {
    this.size = size;
  }
}

// Builder Interface
interface BurgerBuilder {
  addCheese(): this;
  addPepperoni(): this;
  addLettuce(): this;
  build(): Burger;
}

// Concrete Builder
class ConcreteBurgerBuilder implements BurgerBuilder {
  private burger: Burger;

  constructor(size: string) {
    this.burger = new Burger(size);
  }

  addCheese(): this {
    this.burger.cheese = true;
    return this;
  }

  addPepperoni(): this {
    this.burger.pepperoni = true;
    return this;
  }

  addLettuce(): this {
    this.burger.lettuce = true;
    return this;
  }

  build(): Burger {
    return this.burger;
  }
}

// Usage
const burger = new ConcreteBurgerBuilder("Large")
  .addCheese()
  .addLettuce()
  .build();

console.log(burger);
```

---

### **✅ PHP Implementation**
```php
// Product: The complex object we want to build
class Burger {
    public string $size;
    public bool $cheese = false;
    public bool $pepperoni = false;
    public bool $lettuce = false;

    public function __construct(string $size) {
        $this->size = $size;
    }
}

// Builder Interface
interface BurgerBuilder {
    public function addCheese(): self;
    public function addPepperoni(): self;
    public function addLettuce(): self;
    public function build(): Burger;
}

// Concrete Builder
class ConcreteBurgerBuilder implements BurgerBuilder {
    private Burger $burger;

    public function __construct(string $size) {
        $this->burger = new Burger($size);
    }

    public function addCheese(): self {
        $this->burger->cheese = true;
        return $this;
    }

    public function addPepperoni(): self {
        $this->burger->pepperoni = true;
        return $this;
    }

    public function addLettuce(): self {
        $this->burger->lettuce = true;
        return $this;
    }

    public function build(): Burger {
        return $this->burger;
    }
}

// Usage
$burger = (new ConcreteBurgerBuilder("Large"))
    ->addCheese()
    ->addLettuce()
    ->build();

print_r($burger);
```

---

## 🚀 **Director (Optional)**
The **Director** class is useful when you need predefined configurations.

### **TypeScript Director**
```typescript
class BurgerDirector {
  static createVeggieBurger(builder: BurgerBuilder): Burger {
    return builder.addLettuce().build();
  }
}

const veggieBurger = BurgerDirector.createVeggieBurger(new ConcreteBurgerBuilder("Medium"));
console.log(veggieBurger);
```

### **PHP Director**
```php
class BurgerDirector {
    public static function createVeggieBurger(BurgerBuilder $builder): Burger {
        return $builder->addLettuce()->build();
    }
}

$veggieBurger = BurgerDirector::createVeggieBurger(new ConcreteBurgerBuilder("Medium"));
print_r($veggieBurger);
```

---

## 🔥 **Real-World Example: Building a Computer**
Let's take another example: a **computer** with optional parts.

### **TypeScript**
```typescript
class Computer {
  public CPU: string;
  public RAM: string;
  public SSD?: string;
  public GPU?: string;

  constructor(cpu: string, ram: string) {
    this.CPU = cpu;
    this.RAM = ram;
  }
}

class ComputerBuilder {
  private computer: Computer;

  constructor(cpu: string, ram: string) {
    this.computer = new Computer(cpu, ram);
  }

  addSSD(ssd: string): this {
    this.computer.SSD = ssd;
    return this;
  }

  addGPU(gpu: string): this {
    this.computer.GPU = gpu;
    return this;
  }

  build(): Computer {
    return this.computer;
  }
}

// Usage
const gamingPC = new ComputerBuilder("Intel i9", "32GB")
  .addSSD("1TB")
  .addGPU("RTX 4090")
  .build();

console.log(gamingPC);
```

---

### **PHP**
```php
class Computer {
    public string $CPU;
    public string $RAM;
    public ?string $SSD = null;
    public ?string $GPU = null;

    public function __construct(string $CPU, string $RAM) {
        $this->CPU = $CPU;
        $this->RAM = $RAM;
    }
}

class ComputerBuilder {
    private Computer $computer;

    public function __construct(string $CPU, string $RAM) {
        $this->computer = new Computer($CPU, $RAM);
    }

    public function addSSD(string $SSD): self {
        $this->computer->SSD = $SSD;
        return $this;
    }

    public function addGPU(string $GPU): self {
        $this->computer->GPU = $GPU;
        return $this;
    }

    public function build(): Computer {
        return $this->computer;
    }
}

// Usage
$gamingPC = (new ComputerBuilder("Intel i9", "32GB"))
    ->addSSD("1TB")
    ->addGPU("RTX 4090")
    ->build();

print_r($gamingPC);
```

---

## ❌ **Bad Implementations of Builder Pattern**
- **Returning a new instance instead of modifying the existing one.**  
- **Making builder methods static.**  
- **Not following method chaining (`return this`).**  
- **Allowing modifications after calling `.build()`.**  
- **Forcing the user to use a builder when the object is not complex.**  

---

## 🏁 **Conclusion**
✅ The Builder Pattern helps construct complex objects in a **step-by-step manner** while improving readability and maintainability.  


### **📌 Summary of the Builder Design Pattern**

The **Builder Design Pattern** is useful when you need to construct **complex objects step by step**. It allows you to create different variations of an object without cluttering the constructor with too many parameters.

### **🔥 Key Takeaways**
- ✅ **Avoids "Telescoping Constructor" Problem** (too many parameters in the constructor).  
- ✅ **Improves Readability** by allowing method chaining.  
- ✅ **Makes Object Immutable** after `.build()` is called.  
- ✅ **Encapsulates Construction Logic** within a separate builder class.  
- ✅ **Optional `Director` Class** can define predefined configurations.  

### **💡 How to Spot Bad Implementations?**
- ❌ **Returning a new instance instead of modifying the existing one** inside builder methods.  
- ❌ **Making builder methods static**, breaking method chaining.  
- ❌ **Allowing modifications after calling `.build()`**, breaking immutability.  
- ❌ **Forcing a builder when the object isn’t complex enough**, adding unnecessary complexity.  

### **🚀 When to Use It?**
Use the Builder Pattern when:
1. Your object has **many optional parameters**.
2. Your object **requires complex construction** (e.g., setting multiple dependencies).
3. You need to create **different variations of an object** with different configurations.
