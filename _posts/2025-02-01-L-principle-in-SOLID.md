---
title: The L principle in SOLID
excerpt: The "L" in SOLID stands for the Liskov Principle (LCP)
last_modified_at: 2025-02-01T09:45:06-05:00
header: 
teaser: 
tags:
  - SOLID
  - kotlin
---
The **L** in **SOLID** stands for the **Liskov Substitution Principle (LSP)**. It states that:

> **"Objects of a subclass should be replaceable with objects of a superclass without affecting the correctness of the program."**

### Explanation

- A subclass should **extend** the behavior of the parent class, not **break** it.
- If a function expects an object of the parent class, it should work correctly when a subclass object is provided.
- Violating LSP leads to **unexpected behaviors** and breaks polymorphism.

---

### Bad Example (Violating LSP)

Let's say we have a `Rectangle` class:
```kotlin
open class Rectangle(var width: Int, var height: Int) {
	open fun setWidth(w: Int) { 
		width = w 
	}     
	open fun setHeight(h: Int) { 
		height = h 
	}     
	fun area() = width * height 
}
```

Now, we create a `Square` class that **inherits** from `Rectangle`:
```kotlin
class Square(width: Int, height: Int) : Rectangle(width, height) {     
	override fun setWidth(w: Int) {         
		super.setWidth(w)         
		super.setHeight(w)  // Forces height to be the same as width     
	}      
	override fun setHeight(h: Int) {         
		super.setHeight(h)         
		super.setWidth(h)  // Forces width to be the same as height     
	} 
}
```

💡 **Problem:**

- A `Square` is a special type of `Rectangle`, but it **modifies** the expected behavior.
- If a function relies on `Rectangle`, passing a `Square` may cause **unexpected results**.

#### **Example Breaking LSP**
```kotlin
fun printArea(rect: Rectangle) {
    rect.setWidth(5)
    rect.setHeight(10)
    println("Expected area: 50, Actual: ${rect.area()}")
}

val rectangle = Rectangle(4, 5)
printArea(rectangle)  // Output: Expected area: 50, Actual: 50 ✅

val square = Square(4, 4)
printArea(square)  // Output: Expected area: 50, Actual: 100 ❌ (because width and height are always the same)

```

Here, `printArea()` expects a `Rectangle`, but passing a `Square` breaks the logic.

---

### Good Example (Following LSP)

Instead of forcing a `Square` to fit into a `Rectangle`, we should **separate their hierarchy**:
```kotlin
abstract class Shape {
    abstract fun area(): Int
}

class Rectangle(private var width: Int, private var height: Int) : Shape() {
    override fun area() = width * height
}

class Square(private var side: Int) : Shape() {
    override fun area() = side * side
}

```
#### **Usage (LSP is followed)**
```kotlin
fun printArea(shape: Shape) {
    println("Area: ${shape.area()}")
}

val rectangle = Rectangle(5, 10)
val square = Square(5)

printArea(rectangle)  // Output: Area: 50 ✅
printArea(square)  // Output: Area: 25 ✅

```

💡 **Why is this better?**

- `Rectangle` and `Square` both extend `Shape`, but **don't interfere with each other's behavior**.
- The function `printArea()` treats both as `Shape`, maintaining **correct behavior**.

---

### Key Takeaways

- Subclasses should behave as expected when replacing a superclass.  
- Avoid **overriding methods** that change core behaviors.  
- Prefer **composition over inheritance** if behaviors differ.