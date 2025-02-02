---
title: Scope functions in Kotlin
excerpt: In Kotlin, `let`, `apply`, `run`, `also`, and `with` are extension functions that allow you to work with objects in a more concise and readable manner.
last_modified_at: 2025-02-02T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
---

In Kotlin, `let`, `apply`, `run`, `also`, and `with` are extension functions that allow you to work with objects in a more concise and readable manner. They are commonly used for scoping functions, and each one has specific behavior and use cases. Here's a quick breakdown:

### 1. **let**

- **Purpose**: Executes a block of code on the object and returns the result of the block.
- **Receiver**: The object is available as `it`.
- **Return value**: The result of the lambda.
- **Use case**: When you need to perform an operation on an object and return a value (usually for null safety checks or chaining).
```kotlin
val result = "Hello".let {     
	println(it)  // it refers to the object "Hello"     
	it.length    // returns the length of the string 
} 
println(result)  // Output: 5
```
### 2. **apply**

- **Purpose**: Executes a block of code on the object and returns the object itself.
- **Receiver**: The object is available as `this`.
- **Return value**: The object itself (used for object configuration or initialization).
- **Use case**: When you want to configure an object without changing the object reference.
```kotlin
val person = Person().apply {     
	name = "John"     
	age = 30 
} // person is now configured
```
### 3. **run**

- **Purpose**: Executes a block of code and returns the result of the block.
- **Receiver**: The object is available as `this`.
- **Return value**: The result of the lambda.
- **Use case**: When you need to execute a block of code on an object and return a value, similar to `let`, but with access to `this` (the object).
```kotlin
val result = "Hello".run {     
	println(this)  // this refers to the object "Hello"     
	this.length    // returns the length of the string 
} 
println(result)  // Output: 5
```
### 4. **also**

- **Purpose**: Executes a block of code on the object and returns the object itself.
- **Receiver**: The object is available as `it`.
- **Return value**: The object itself (used when you want to perform some action but keep the object unchanged).
- **Use case**: When you want to perform additional actions on an object but still use the object itself.
```kotlin
val person = Person().also {     
	println("Person's name: ${it.name}")  // it refers to the object 
}
```
### 5. **with**

- **Purpose**: Executes a block of code on an object and returns the result of the block.
- **Receiver**: The object is available as `this`.
- **Return value**: The result of the lambda.
- **Use case**: When you want to operate on an object without modifying it and return a result, but you don't need to chain actions.
```kotlin
val result = with(person) {     
	name = "Jane"     
	age = 25     
	"Name: $name, Age: $age"  // returns this string 
} 
println(result)  // Output: Name: Jane, Age: 25
```
### Summary of Differences:

|Function|Receiver|Return Value|Use Case|
|---|---|---|---|
|`let`|`it`|Result of block|Transform or process object and return a result|
|`apply`|`this`|The object itself|Configure or initialize the object|
|`run`|`this`|Result of block|Execute block on the object and return a result|
|`also`|`it`|The object itself|Perform actions on object without changing it|
|`with`|`this`|Result of block|Operate on an object and return a result without chaining|

These functions can be very useful in Kotlin for scoping operations and improving code readability!