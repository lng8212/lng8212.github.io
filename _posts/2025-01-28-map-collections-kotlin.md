---
title: Map Collections in Kotlin
excerpt: Map collection explaination.
last_modified_at: 2025-01-28T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
  - collection
---
In Kotlin, there are primarily **two types of maps**:

1. **`Map` (Immutable)**:
    
    - A read-only map that cannot be modified after creation.
    - You can access elements but cannot add, remove, or modify them.
2. **`MutableMap`**:
    
    - A modifiable map where you can add, update, or remove key-value pairs.

Note: What is internally happen when we call `mapOf()` or `mutableMapOf()` in Kotlin?

```kotlin
// When you write this:
val fruits = mapOf("apple" to 1, "banana" to 2)

// Internally, Kotlin first creates a LinkedHashMap
val internalMap = LinkedHashMap<String, Int>()
internalMap.put("apple", 1)
internalMap.put("banana", 2)

// Then it wraps it in an immutable view. If fruits is mutable, below line is not appeared.
val fruits = Collections.unmodifiableMap(internalMap)
```


---

### **Subtypes of `Map` and `MutableMap`**

Kotlin’s `Map` and `MutableMap` are interfaces, and their actual implementations come from the Java collections framework. Here are the common implementations:

#### 1. **HashMap** (backed by Java's `HashMap`)

- Unordered collection.
- Fast for most general use cases.
- Key lookup, insertion, and deletion are average **O(1)**.
- Example:
```kotlin
val hashMap: MutableMap<Int, String> = HashMap() 
	hashMap[1] = "One" 
    hashMap[2] = "Two" 
    println(hashMap) 
	// Output: {1=One, 2=Two}
```
#### 2. **LinkedHashMap** (backed by Java's `LinkedHashMap`)

- Maintains insertion order of key-value pairs.
- Slightly slower than `HashMap` due to the overhead of maintaining order.
- Example:
```kotlin
val linkedHashMap: MutableMap<Int, String> = LinkedHashMap() 
	linkedHashMap[1] = "One" 
    linkedHashMap[2] = "Two" 
	println(linkedHashMap) // Output: {1=One, 2=Two}`
```

#### 3. **TreeMap** (backed by Java's `TreeMap`)

- Keys are sorted in natural order or by a custom comparator.
- Slower than `HashMap` due to the sorting overhead.
- Useful when you need sorted keys.
- Example:
```kotlin
val treeMap: MutableMap<Int, String> = java.util.TreeMap() 
treeMap[2] = "Two" 
treeMap[1] = "One" 
println(treeMap) // Output: {1=One, 2=Two}
```
#### 4. **ConcurrentHashMap** (backed by Java's `ConcurrentHashMap`)

- Thread-safe map designed for concurrent use.
- High performance in multi-threaded environments.
- Example:
```kotlin
val concurrentHashMap = java.util.concurrent.ConcurrentHashMap<Int, String>() 
concurrentHashMap[1] = "One" 
concurrentHashMap[2] = "Two" 
println(concurrentHashMap) // Output: {1=One, 2=Two}
```

---

### **How to Create a Map in Kotlin**

#### 1. **Immutable Map**
```kotlin
val map: Map<Int, String> = mapOf(1 to "One", 2 to "Two") 
println(map[1]) // Output: One 
// map[3] = "Three" 
// Compilation error (cannot modify an immutable map)
```
#### 2. **Mutable Map**
```kotlin
val mutableMap: MutableMap<Int, String> = mutableMapOf(1 to "One", 2 to "Two") 
mutableMap[3] = "Three" 
println(mutableMap) // Output: {1=One, 2=Two, 3=Three}
```

---

### **Comparison Table**

|**Type**|**Modifiable**|**Order Maintained**|**Thread-Safe**|**Key Sorting**|**Average Lookup Time**|
|---|---|---|---|---|---|
|`HashMap`|Yes|No|No|No|O(1)|
|`LinkedHashMap`|Yes|Yes (Insertion order)|No|No|O(1)|
|`TreeMap`|Yes|Yes (Sorted order)|No|Yes|O(log n)|
|`ConcurrentHashMap`|Yes|No|Yes|No|O(1)|
|`Map`|No|Depends on implementation|No|Depends|Depends|
|`MutableMap`|Yes|Depends on implementation|No|Depends|Depends|

---

### **When to Use Which Map?**

- **`HashMap`**: Default choice for general-purpose maps (fast and efficient).
- **`LinkedHashMap`**: When you need to maintain the insertion order.
- **`TreeMap`**: When keys must be sorted.
- **`ConcurrentHashMap`**: When working with multi-threaded environments.
- **`Map`/`MutableMap`**: Use these interfaces when you want to abstract away the implementation details.