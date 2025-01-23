---
title: Data class and class in Kotlin
excerpt: The difference of data class and class in Kotlin
last_modified_at: 2025-01-23T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
  - class
toc: true
---

In Kotlin, both `data class` and `class` are used to define classes, but they serve different purposes and come with different features. Here's a detailed comparison:

---

## **1. Class (`class`)**

A regular class in Kotlin is a blueprint for creating objects, with no additional functionality provided by default. You need to implement properties and methods manually.

### **Features**

- **Custom Behavior:** You can define any kind of behavior, methods, or properties as needed.
- **No Auto-Generated Functions:** Functions like `equals()`, `hashCode()`, `toString()`, and `copy()` are not automatically generated.
- Best for classes with significant logic or behavior.

### **Example**

```kotlin
class Person(val name: String, val age: Int)

fun main() {
    val person1 = Person("Alice", 25)
    val person2 = Person("Alice", 25)

    // Default behavior
    println(person1.toString()) // Output: Person@1b6d3586
    println(person1 == person2) // Output: false (compares references)
}
```

---

## **2. Data Class (`data class`)**

A `data class` in Kotlin is specifically designed to hold data. It provides several useful functions out of the box to reduce boilerplate code.

### **Features**

- **Auto-Generated Functions:**
    - `equals()` (compares content)
    - `hashCode()` (hash based on properties)
    - `toString()` (human-readable representation)
    - `copy()` (create a copy with optional property modifications)
- **Immutable by Default:** `val` is often used for properties to ensure immutability.
- Best for classes meant to store and manipulate data (e.g., DTOs, POJOs).

### **Example**

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val person1 = Person("Alice", 25)
    val person2 = Person("Alice", 25)

    // Auto-generated behavior
    println(person1.toString()) // Output: Person(name=Alice, age=25)
    println(person1 == person2) // Output: true (compares content)

    // Copy with modification
    val person3 = person1.copy(age = 26)
    println(person3) // Output: Person(name=Alice, age=26)
}
```

---

## **Key Differences**

|Feature|`class`|`data class`|
|---|---|---|
|**Purpose**|General purpose|Storing and manipulating data|
|**Equals Comparison**|Reference-based|Content-based|
|**HashCode**|Not auto-generated|Auto-generated based on content|
|**ToString**|Default implementation|Readable representation|
|**Copy**|Not available by default|`copy()` provided|
|**Use Case**|For logic and behaviors|For data models or DTOs|

---

## **When to Use Which?**

- Use **`class`** when:
    - You need more complex behavior, logic, or mutable states.
    - The class is not primarily designed to hold data.
- Use **`data class`** when:
    - The primary purpose is to store and manipulate data.
    - You need equality checks, `toString()`, and `copy()` functionality without writing boilerplate code.

---