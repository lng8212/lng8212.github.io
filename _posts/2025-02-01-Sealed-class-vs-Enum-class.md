---
title: Sealed class vs Enum class in Kotlin
excerpt: Both **sealed classes** and **enums** in Kotlin are used to represent a **fixed set of types**, but they serve different purposes and have distinct features.
last_modified_at: 2025-02-01T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
---

Both **sealed classes** and **enums** in Kotlin are used to represent a **fixed set of types**, but they serve different purposes and have distinct features.

---

### **1. `enum` (Enumeration)**

An `enum` is used when you have a **finite set of constant values** that do not change.

#### **Key Features of `enum`**

✔ Represents **fixed constants**.  
✔ Each `enum` value is a **single instance (singleton)**.  
✔ Can have properties and methods.  
✔ Cannot store additional hierarchy (i.e., no subclasses).

#### **Example:**
```kotlin
enum class CarType(val speed: Int) {     
	SEDAN(180),     
	SUV(160),     
	SPORTS(250);      
	fun printSpeed() {         
		println("Max speed: $speed km/h")     
	} 
}  

fun main() {     
	val car = CarType.SPORTS     
	car.printSpeed()  // Output: Max speed: 250 km/h 
}
```

💡 **Use `enum` when you need predefined, fixed constants with optional properties/methods**.

---

### **2. `sealed class`**

A `sealed class` is used when you need to define a **restricted class hierarchy**, where each subclass represents a specific state or behavior.

#### **Key Features of `sealed class`**

✔ Can have **multiple subclasses** (unlike `enum`).  
✔ Useful for **representing different types of states** (e.g., Success, Error).  
✔ Can store additional properties for each subclass.  
✔ More **flexible** than `enum`.

#### **Example:**
```kotlin
sealed class Car {
    data class Sedan(val speed: Int) : Car()
    data class SUV(val speed: Int, val offRoad: Boolean) : Car()
    data class Sports(val speed: Int, val turbo: Boolean) : Car()
}

fun carInfo(car: Car) {
    when (car) {
        is Car.Sedan -> println("Sedan speed: ${car.speed} km/h")
        is Car.SUV -> println("SUV speed: ${car.speed}, Off-road: ${car.offRoad}")
        is Car.Sports -> println("Sports car speed: ${car.speed}, Turbo: ${car.turbo}")
    }
}

fun main() {
    val myCar = Car.Sports(300, true)
    carInfo(myCar)  // Output: Sports car speed: 300, Turbo: true
}

```

💡 **Use `sealed class` when you need to define multiple subclasses with different properties and behavior.**

---
### **Key Differences:**

| Feature                                 | `enum` ✅                         | `sealed class` ✅                       |
| --------------------------------------- | -------------------------------- | -------------------------------------- |
| Represents fixed set of types?          | ✅ Yes                            | ✅ Yes                                  |
| Can have different properties per type? | ❌ No (all must have the same)    | ✅ Yes                                  |
| Can have methods?                       | ✅ Yes                            | ✅ Yes                                  |
| Can have multiple instances?            | ❌ No (each value is a singleton) | ✅ Yes (can create multiple instances)  |
| Can extend into subclasses?             | ❌ No                             | ✅ Yes                                  |
| Best for?                               | **Fixed constant values**        | **Hierarchical states & polymorphism** |

---
### **When to Use What?**

**Use `enum` when**

- You have a **finite set of constant values**.
- All values have the **same properties and behavior**.
- Example: `Color.RED`, `LogLevel.ERROR`, `CarType.SUV`.

**Use `sealed class` when**

- You need a **restricted class hierarchy**.
- Each subclass has **different properties or behavior**.
- Example: **Success/Failure states**, **different UI states** in Android.