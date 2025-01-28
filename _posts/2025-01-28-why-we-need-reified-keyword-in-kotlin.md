---
title: Why we need reified keyword in Kotlin
excerpt: In Kotlin, the reified keyword is used with inline functions to retain the type information at runtime.
last_modified_at: 2025-01-28T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
---

In Kotlin, the **`reified`** keyword is used with **inline functions** to retain the type information at runtime. Normally, due to **type erasure** in Kotlin and Java, the type information of generic classes is not available at runtime. The `reified` keyword solves this by embedding the type information into the bytecode of inline functions.

This is especially useful when working with libraries like **Gson**, where you need to deserialize JSON into an object of a specific type. Gson requires the type information to parse the JSON correctly, but type erasure makes it unavailable.

---

### **Why Do We Need `reified`?**

Consider the following example where we use **Gson** to parse JSON into an object:
```kotlin
val json = """{ "name": "John", "age": 30 }""" 
// Deserializing JSON without reified 
val person = Gson().fromJson(json, Person::class.java)
```
Here, the type **`Person`** is explicitly provided using `Person::class.java`.

If you try to create a generic function to deserialize any type:
```kotlin
fun <T> fromJson(json: String): T {     
	return Gson().fromJson(json, T::class.java) // ERROR: Cannot use 'T::class' 
}
```

This won't work because **type `T` is erased at runtime**. You cannot use `T::class` or `T::class.java` directly without passing the type information explicitly.

---
### **Solution with `reified`**

The **`reified`** keyword allows you to retain the type information for `T` at runtime in inline functions. Here's how it works:
```kotlin
inline fun <reified T> fromJson(json: String): T {     
	return Gson().fromJson(json, T::class.java) 
}
```

Now you can call the function without manually passing the type:
```kotlin
val json = """{ "name": "John", "age": 30 }""" 
val person: Person = fromJson(json) // Works without T::class.java 
println(person) // Output: Person(name=John, age=30)
```

---

### **How It Works**

1. **Inline Function**:
    
    - The function is marked with `inline`, meaning the compiler inlines the function's body where it's called.
    - This allows the compiler to replace `T` with the actual type.
2. **Reified Keyword**:

    - The type `T` becomes available at runtime, allowing operations like `T::class.java`.

---

### **Example: Gson and a Generic API Response**

Imagine you're parsing an API response where the JSON format varies for different data types. Using `reified` helps simplify this:

#### JSON String:
```json
{ "data": { "name": "Alice", "age": 25 } }
```

#### Kotlin Code:
```kotlin
data class ApiResponse<T>(val data: T)

data class Person(val name: String, val age: Int)  

inline fun <reified T> parseApiResponse(json: String): ApiResponse<T> {     
	val type = object : TypeToken<ApiResponse<T>>() {}.type     
	return Gson().fromJson(json, type) 
}  // Usage val json = """{ "data": { "name": "Alice", "age": 25 } }""" 
val response: ApiResponse<Person> = parseApiResponse(json) println(response.data) // Output: Person(name=Alice, age=25)`
```

---

### **Advantages of `reified`**

1. **Simplifies Generic Code**:
    - No need to pass `Class<T>` or `Type` manually.
2. **Type Safety**:
    - Retains type information at runtime, preventing casting issues.
3. **Readability**:
    - Cleaner code, especially when dealing with complex generics.

---

### **Limitations**

- Only works with **inline functions**.
- Cannot be used in regular functions due to type erasure.

---

### **Summary**

The `reified` keyword is essential in Kotlin for retaining type information at runtime in generic code. It shines when used with libraries like Gson, enabling seamless JSON deserialization without additional boilerplate or manual type specification.