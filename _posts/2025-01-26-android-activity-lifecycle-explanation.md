---
title: Android Activity Lifecycle Explanation
excerpt: The short explanation about Android fragment
last_modified_at: 2025-01-26T07:45:06-05:00
header: 
teaser: 
tags:
  - lifecycle
  - android
---

The Android Activity lifecycle consists of various states that an Activity transitions through during its lifetime. These states allow the system to manage resources effectively and provide a seamless user experience. Here's an overview of the lifecycle and its callback methods:

---

### **Lifecycle Callbacks**

1. **onCreate()**:
    
    - Triggered when the Activity is first created.
    - Used to initialize the Activity, set up the UI, and prepare resources like views or data binding.
    - Called **once** during the Activity's lifetime unless the system destroys it and recreates it (e.g., configuration changes).
    
    Example:
    
    ```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)     
        setContentView(R.layout.activity_main) 
    }
    ```
    
2. **onStart()**:
    
    - Called when the Activity becomes visible to the user but is not yet interactive.
    - The Activity enters the **"Started"** state.
    - Often used for preparing UI elements or refreshing data.
3. **onResume()**:
    
    - Triggered when the Activity comes to the foreground and is fully interactive.
    - The Activity is in the **"Resumed"** state, also referred to as the **Running** state.
    - Handle real-time updates, user interactions, and animations here.
4. **onPause()**:
    
    - Called when the Activity is partially obscured but still visible (e.g., another Activity appears in front of it as a dialog).
    - Release resources that don’t need to run when the Activity is not in the foreground, such as stopping animations or pausing media playback.
5. **onStop()**:
    
    - Triggered when the Activity is no longer visible (e.g., when navigating to another Activity or pressing the home button).
    - Use this to release or save resources like database connections or network calls.
6. **onRestart()**:
    
    - Called after the Activity has been stopped and is about to restart.
    - Typically followed by **onStart()**.
    - Useful for reinitializing components if necessary.
7. **onDestroy()**:
    
    - Called before the Activity is destroyed, either because the user finished it or the system is reclaiming memory.
    - Clean up resources (e.g., unregister listeners, close database connections) to avoid memory leaks.

---

### **Lifecycle Flow**

#### **Normal Flow**:

1. **onCreate() → onStart() → onResume()**
    - The Activity is created, becomes visible, and is ready for user interaction.
#### **Interrupted Flow**:

2. **onPause() → onStop()**
    - The Activity is no longer in the foreground or visible (e.g., user navigates away or the system interrupts).

#### **Restart Flow**:

3. **onRestart() → onStart()**
    - When the Activity is returning to the foreground after being stopped.

#### **End of Lifecycle**:

4. **onDestroy()**
    - The Activity is destroyed and removed from memory.

---

### **Visual Representation**
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/Android-Activity-Lifecycle.png" alt="">
</figure> 
---

### **Key Notes**

- **onCreate()** is called only once unless the Activity is destroyed.
- **onPause()** is the first callback triggered when the user leaves the Activity.
- Use **onSaveInstanceState()** to save UI state before the Activity is destroyed or stopped.
- **onDestroy()** is the final cleanup step; ensure resources are released here.

This structured approach ensures that the system manages resources efficiently and provides a responsive user experience.

