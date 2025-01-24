---
title: Interface vs Abstract Class and Open Class in Kotlin
excerpt: In Kotlin, interfaces, abstract classes, and open classes are powerful constructs used for defining behavior and class hierarchies. Each has specific features and use cases that cater to different design requirements.
last_modified_at: 2025-01-23T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
  - class
toc: true
---

In Kotlin, **interfaces**, **abstract classes**, and **open classes** are powerful constructs used for defining behavior and class hierarchies. Each has specific features and use cases that cater to different design requirements. Here's a detailed summary:

---

## **1. Interface**

An **interface** in Kotlin defines a contract that a class can implement. It is primarily used to define behavior that can be shared across unrelated classes.
### **Backing field and initial values:**
First of all, we need to know about backing field and why interfaces do not have that function.

In Kotlin, a **backing field** refers to the underlying field that stores the value of a property. This field is automatically generated for properties that have custom getters and setters. It's used when you need to store the value of a property internally and access it through a getter or setter. Backing fields are primarily used in **classes** to implement custom accessors, but **you cannot define a backing field in an interface**. 

In a **class**, you can define a backing field using the `field` keyword within a custom getter or setter. This field is automatically generated for properties that need a custom getter or setter.
Example:
```kotlin
class Example { 
// Backing field for the property 
	var name: String = "default" 
	    get() = field.toUpperCase() // custom getter 
	    set(value) { field = value.toLowerCase() } // custom setter 
}
```
Interfaces only define **abstract properties** (without the backing field), and they don't store any state. All properties in an interface are implicitly abstract and must be implemented by the classes that inherit from the interface.

**Interfaces and Properties**:

```Kotlin
interface MyInterface {
    // Property with a default value
    val name: String
        get() = "Default Name"  // Default implementation in the interface
}

class MyClass : MyInterface {
    // You can override the default value here if needed
    override val name: String = "MyClass Name"
}

fun main() {
    val myClass = MyClass()
    println(myClass.name)  // Output: MyClass Name
}

```

-   **State Storage**: While interfaces can provide default values for properties via **getter implementations**, they do **not** maintain or store the value themselves. The actual **state** (the value) is stored in the class that implements the interface.
    
-   **Initialization in Interfaces**: You can indeed **initialize** a property in an interface, but this initialization is provided through a getter (not a backing field). The interface will define a **default behavior** (e.g., returning a value), but the actual property value is **managed by the class** that implements the interface.
### **Key Features:**

- **Multiple Inheritance:** A class can implement multiple interfaces, providing flexibility in design.

- **Abstract and Concrete Methods:** Interfaces can have abstract methods (without a body) and concrete methods (with a default implementation).
    
- **Properties:** Interfaces can declare abstract properties or provide default implementations. However, they cannot have a backing field.
    
- **No State:** Interfaces cannot hold state, meaning properties in an interface are stateless and don’t have backing fields.
    
- **Default Methods:** Interfaces can provide default implementations for methods using the `fun` keyword.
    

### **Use Cases:**

- When you need to define shared behavior for unrelated classes.
    
- When multiple inheritance is required.
    
- To ensure a consistent contract for implementing classes.
    

### **Example:**

```kotlin
interface Animal {
    val species: String // Abstract property
    fun sound(): String // Abstract method

    fun sleep() { // Default method
        println("The animal is sleeping.")
    }
}

class Dog : Animal {
    override val species = "Dog"
    override fun sound() = "Bark"
}

fun main() {
    val dog = Dog()
    println(dog.species) // Output: Dog
    println(dog.sound()) // Output: Bark
    dog.sleep()          // Output: The animal is sleeping.
}
```

---

## **2. Abstract Class**

An **abstract class** in Kotlin serves as a blueprint for a group of related classes. It provides a way to define both shared behavior and mandatory functionality that must be implemented by subclasses.

### **Key Features:**

- **Abstract Methods:** Can define methods without a body that must be implemented by subclasses.
    
- **Concrete Methods:** Can have methods with default implementations.
    
- **Stateful:** Abstract classes can hold state using properties with backing fields.
    
- **Constructors:** Abstract classes can have constructors to initialize properties or enforce specific setups.
    
- **Single Inheritance:** A class can inherit from only one abstract class.
    

### **Use Cases:**

- When you want to provide a base class with common functionality and shared state.
    
- When a class hierarchy is clear and tightly related.
    
- To ensure specific methods or properties are implemented by subclasses.
    

### **Example:**

```kotlin
abstract class Animal(val species: String) {
    abstract fun sound(): String // Abstract method

    fun sleep() { // Concrete method
        println("The $species is sleeping.")
    }
}

class Dog : Animal("Dog") {
    override fun sound() = "Bark"
}

fun main() {
    val dog = Dog()
    println(dog.species) // Output: Dog
    println(dog.sound()) // Output: Bark
    dog.sleep()          // Output: The Dog is sleeping.
}
```

---

## **3. Open Class**

An **open class** in Kotlin is a class that can be inherited from. Unlike abstract classes, it can be instantiated directly and does not enforce any implementation in subclasses.

### **Key Features:**

- **Open for Inheritance:** By default, classes in Kotlin are `final` (cannot be inherited). Adding the `open` keyword allows a class to be extended.
    
- **Concrete Methods:** Methods in an open class are concrete by default. You must explicitly mark methods as `open` to allow overriding.
    
- **Stateful:** Like abstract classes, open classes can hold state and define properties with backing fields.
    
- **Constructors:** Open classes can have primary and secondary constructors for initializing properties.
    
- **Single Inheritance:** A class can inherit from only one open class.
    

### **Use Cases:**

- When you want a base class to provide optional overrides without forcing implementation.
    
- When the base class can stand alone but still allows for extension in specific scenarios.
    

### **Example:**

```kotlin
open class Animal(val species: String) {
    open fun sound(): String {
        return "Some generic sound"
    }

    fun sleep() {
        println("The $species is sleeping.")
    }
}

class Dog : Animal("Dog") {
    override fun sound(): String {
        return "Bark"
    }
}

fun main() {
    val animal = Animal("Generic Animal")
    println(animal.sound()) // Output: Some generic sound

    val dog = Dog()
    println(dog.sound()) // Output: Bark
    dog.sleep()          // Output: The Dog is sleeping.
}
```

---

## **Key Differences: Interface vs Abstract Class vs Open Class**

|Feature|**Interface**|**Abstract Class**|**Open Class**|
|---|---|---|---|
|**Instantiation**|Cannot be instantiated|Cannot be instantiated|Can be instantiated|
|**Purpose**|Defines a contract for implementation|Serves as a base class with shared behavior|Allows inheritance with optional overrides|
|**Inheritance**|Multiple interfaces can be implemented|Single inheritance|Single inheritance|
|**State**|Cannot hold state|Can hold state|Can hold state|
|**Default Methods**|Supported|Supported|Supported|
|**Constructors**|Not allowed|Allowed|Allowed|
|**Use Case**|For defining behavior across unrelated classes|For shared state and behavior in related classes|For optional overrides in related classes|

---

## **When to Use Each?**

- **Use an Interface** when:
    
    - You need to define shared behavior for unrelated classes.
        
    - Multiple inheritance is required.
        
    - No shared state or constructors are needed.
        
- **Use an Abstract Class** when:
    
    - You want to define a base class with shared state or functionality.
        
    - Subclasses must implement specific methods or properties.
        
    - A clear class hierarchy exists.
        
- **Use an Open Class** when:
    
    - You want a class to allow inheritance with optional overrides.
        
    - The class can stand alone and doesn’t enforce subclass implementation.
