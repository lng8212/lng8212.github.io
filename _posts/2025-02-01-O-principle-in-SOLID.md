---
title: The O principle in SOLID
excerpt: The "O" in SOLID stands for the Open/Closed Principle (OCP)
last_modified_at: 2025-02-01T09:45:06-05:00
header: 
teaser: 
tags:
  - SOLID
  - kotlin
---
The "O" in **SOLID** stands for the **Open/Closed Principle (OCP)**. This principle states that:

> **"Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification."**

### Explanation

- **Open for extension** → You should be able to add new functionality without changing existing code.
- **Closed for modification** → Once a class is developed and tested, you should not modify it directly. Instead, extend it using inheritance, interfaces, or other design patterns.

### Why is OCP Important?

- Avoids modifying tested and working code, reducing bugs.
- Enhances maintainability and scalability.
- Supports adding new features without breaking existing functionality.

---

### Example in Kotlin

❌ **Bad Example (Violating OCP)**  
Let's say we have a `DiscountCalculator` class that calculates a discount based on the user type:
```kotlin
class DiscountCalculator {     
	fun calculateDiscount(userType: String, price: Double): Double {   
		return when (userType) {             
			"Regular" -> price * 0.1  // 10% discount             
			"Premium" -> price * 0.2  // 20% discount             
			else -> 0.0         
		}     
	} 
}
```

💡 **Problem:**

- If we need to add a new user type (e.g., "VIP" with a 30% discount), we must modify `calculateDiscount()`, violating OCP.
- Frequent modifications increase the risk of bugs.

---

✅ **Good Example (Following OCP)**  
A better approach is to **use polymorphism** by creating an abstract class or interface:
```kotlin
interface DiscountStrategy {
    fun applyDiscount(price: Double): Double
}

class RegularDiscount : DiscountStrategy {
    override fun applyDiscount(price: Double) = price * 0.1
}

class PremiumDiscount : DiscountStrategy {
    override fun applyDiscount(price: Double) = price * 0.2
}

class VIPDiscount : DiscountStrategy {
    override fun applyDiscount(price: Double) = price * 0.3
}

class DiscountCalculator(private val discountStrategy: DiscountStrategy) {
    fun calculateDiscount(price: Double): Double {
        return discountStrategy.applyDiscount(price)
    }
}

```

### **Usage:**
```kotlin
val discountCalculator = DiscountCalculator(PremiumDiscount())
println(discountCalculator.calculateDiscount(100.0))  // Output: 20.0
```
💡 **Why is this better?**

- **Open for extension** → We can add new discount types (e.g., `SuperVIPDiscount`) without modifying existing code.
- **Closed for modification** → The `DiscountCalculator` class does not need to change when new discounts are added.

---

### Key Takeaways

- Avoid modifying existing code; instead, extend functionality using abstraction.  
- Use **interfaces, abstract classes, and polymorphism** to follow OCP.  
- Helps in **scalability and maintainability** of software.