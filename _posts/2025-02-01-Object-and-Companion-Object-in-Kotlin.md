---
title: Object and Companion Object in Kotlin
excerpt: In Kotlin, both `companion object` and `object` classes are used to define singleton-like behavior, but they have different use cases and characteristics.
last_modified_at: 2025-02-01T09:45:06-05:00
header: 
teaser: 
tags:
  - object
  - kotlin
---

In Kotlin, both `companion object` and `object` classes are used to define singleton-like behavior, but they have different use cases and characteristics.

---

###  **1. `companion object`**

A `companion object` is an object inside a **class**, allowing you to define **static-like properties and methods**.

#### **Key Features:**

✔ Belongs to a **specific class**.  
✔ Can access **private members** of the outer class.  
✔ Acts like **static methods** in Java but still respects Kotlin's object-oriented model.  
✔ Only **one instance** is created per class (singleton).

#### **Example:**
```kotlin
class MyClass {
    companion object {
        fun greet() = "Hello from Companion Object"
    }
}

fun main() {
    println(MyClass.greet())  // No need to create an instance of MyClass
}
```

💡 **Equivalent to Java's static methods** but with more flexibility.

---

### **2. `object` Class**

An `object` class in Kotlin **creates a singleton instance directly**.

#### **Key Features:**

✔ It is **self-contained** and does **not** belong to another class.  
✔ Can be used for **utility functions**, **single-instance services**, or **global state**.  
✔ The **object is created lazily** when first accessed.

#### **Example:**
```kotlin
object Singleton {     
	fun greet() = "Hello from Object Class" }  
	fun main() {     
	println(Singleton.greet())  // Directly calling the object 
}
```

💡 **Singleton design pattern** is automatically implemented.

---

### **Key Differences:**

|Feature|`companion object`|`object` class|
|---|---|---|
|Belongs to|A **specific class**|**Standalone** (Global Singleton)|
|Instance Creation|**One per class**|**One per program**|
|Use Case|**Static-like behavior** for a class|**Single-instance utility/service**|
|Access to Outer Class|✅ Yes (Can access private members)|❌ No (It’s independent)|

---

### **When to Use What?**

Use `companion object` when:

- You need **static-like behavior** inside a class (e.g., factory methods).
- You need access to **private properties** of the outer class.

Use `object` class when:

- You need a **singleton** to manage global state or shared resources.
- You need a **utility class** with helper functions.

---

### **Example Comparing Both**
```kotlin
class Car(val name: String) {
    companion object {
        fun createTesla() = Car("Tesla")  // Factory method
    }
}

object CarFactory {
    fun createFord() = Car("Ford")  // Singleton managing object creation
}

fun main() {
    val car1 = Car.createTesla()  // Using companion object
    val car2 = CarFactory.createFord()  // Using object class

    println(car1.name)  // Output: Tesla
    println(car2.name)  // Output: Ford
}
```