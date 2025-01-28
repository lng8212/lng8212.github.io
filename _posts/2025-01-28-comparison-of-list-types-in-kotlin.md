---
title: Comparison of ArrayList, LinkedList, MutableList, List, Array, and Vector in Kotlin
excerpt: A detailed comparison of `ArrayList`, `LinkedList`, `MutableList`, `List`, `Array`, and `Vector` in Kotlin, along with examples and key differences
last_modified_at: 2025-01-28T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
  - collection
---
Here's a detailed comparison of `ArrayList`, `LinkedList`, `MutableList`, `List`, `Array`, and `Vector` in Kotlin, along with examples and key differences:
---

### 1. **`ArrayList`**

- **Type**: Mutable, backed by a dynamically resizable array.
- **Use Case**: When frequent random access is required and the size of the collection might change.
- **Performance**:
    - Access: **O(1)** (fast random access).
    - Insert/Delete: **O(n)** (due to shifting elements).
- **Example**:
```kotlin
 val arrayList = ArrayList<Int>() 
 arrayList.add(1) 
 arrayList.add(2) 
 println(arrayList) // Output: [1, 2]
```
---

### 2. **`LinkedList`**

- **Type**: Mutable, doubly linked list implementation.
- **Use Case**: When frequent insertions or deletions in the middle of the list are required.
- **Performance**:
    - Access: **O(n)** (no random access).
    - Insert/Delete: **O(1)** for adding/removing elements at the head or tail.
- **Example**:
```kotlin
    val linkedList = java.util.LinkedList<Int>() // Use Java's LinkedList
    linkedList.add(1) 
    linkedList.add(2) 
    println(linkedList) // Output: [1, 2]
```
---

### 3. **`MutableList`**

- **Type**: Interface, represents a mutable collection of elements.
- **Use Case**: When you want to modify the list (add/remove elements). Can be implemented by `ArrayList` or `LinkedList`.
- **Performance**: Depends on the underlying implementation (e.g., `ArrayList`, `LinkedList`).
- **Example**:
```kotlin
val mutableList: MutableList<Int> = mutableListOf(1, 2, 3)
mutableList.add(4) 
println(mutableList) // Output: [1, 2, 3, 4]
```
---

### 4. **`List`**

- **Type**: Interface, represents an immutable collection of elements.
- **Use Case**: When you don't want to modify the list after creation.
- **Performance**: Depends on the underlying implementation (e.g., `ArrayList`, `LinkedList`).
- **Example**:
```kotlin
val list: List<Int> = listOf(1, 2, 3) // list.add(4) // Compilation error: Cannot add to an immutable list println(list) // Output: [1, 2, 3]
```
---

### 5. **`Array`**

- **Type**: Fixed-size, strongly-typed collection of elements.
- **Use Case**: When you know the size of the collection at compile time and don't need resizing.
- **Performance**:
    - Access: **O(1)** (fast random access).
    - Insert/Delete: **O(n)** (no dynamic resizing).
- **Example**:
```kotlin
val array = arrayOf(1, 2, 3) 
array[0] = 10 
println(array.joinToString()) // Output: 10, 2, 3
```
---

### 6. **`Vector`**

- **Type**: Thread-safe mutable collection, backed by a dynamically resizable array (from Java).
- **Use Case**: When thread-safe operations on a collection are required.
- **Performance**:
    - Access: **O(1)** (fast random access).
    - Insert/Delete: **O(n)** (similar to `ArrayList`).
    - Slower than `ArrayList` due to synchronization overhead.
- **Example**:
```kotlin
val vector = java.util.Vector<Int>() 
vector.add(1) 
vector.add(2) 
println(vector) // Output: [1, 2]
```
---

### Comparison Table

| **Type**      | **Mutability** | **Thread-Safe** | **Access Performance**    | **Insert/Delete Performance** | **Resizable** | **Fixed-Size** |
| ------------- | -------------- | --------------- | ------------------------- | ----------------------------- | ------------- | -------------- |
| `ArrayList`   | Mutable        | No              | O(1)                      | O(n)                          | Yes           | No             |
| `LinkedList`  | Mutable        | No              | O(n)                      | O(1) for head/tail            | Yes           | No             |
| `MutableList` | Mutable        | No              | Depends on implementation | Depends on implementation     | Yes           | No             |
| `List`        | Immutable      | No              | Depends on implementation | Immutable (no modification)   | Yes           | No             |
| `Array`       | Mutable        | No              | O(1)                      | O(n)                          | No            | Yes            |
| `Vector`      | Mutable        | Yes             | O(1)                      | O(n)                          | Yes           | No             |

---

### Key Points:

- Use **`ArrayList`** for most general-purpose collections when random access is needed.
- Use **`LinkedList`** when frequent insertions and deletions are required.
- Use **`MutableList`** or **`List`** based on whether you need mutability.
- Use **`Array`** for fixed-size collections with better performance.
- Use **`Vector`** for thread-safe collection needs.

**Note**: List and MutableList cover almost all use cases in Kotlin. Their design aligns perfectly with Kotlin's philosophy of simplicity and immutability by default. Unless you have a very specific requirement (e.g., low-level optimizations, thread-safety, or constant insertion/deletion at specific positions), these two are enough.