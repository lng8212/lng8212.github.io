---
title: Android Activity Lifecycle Explanation
excerpt: The short explanation about Android activ
last_modified_at: 2025-01-26T13:45:06-05:00
header: 
teaser: 
tags:
  - lifecycle
  - android
---

A **Fragment** in Android is a reusable portion of the UI associated with an Activity. It has its own lifecycle that closely integrates with the Activity's lifecycle. This allows fragments to manage their UI and behavior efficiently.

---

### **Fragment Lifecycle Callbacks**

1. **onAttach(Context context)**:
    
    - Called when the fragment is associated with an Activity.
    - Provides the **Context** of the host Activity.
    - Perform operations that require the Activity context.
2. **onCreate(Bundle savedInstanceState)**:
    
    - Triggered when the fragment is first created.
    - Initialize non-UI components like variables, ViewModels, or data structures.
    - Similar to an Activity's `onCreate()` but does not inflate the layout.
3. **onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)**:
    
    - Called to inflate the fragment's UI layout.
    - Return the root `View` of the fragment's layout.
    
    Example:
    
    ```kotlin
    
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState:Bundle?): View? {     
        return inflater.inflate(R.layout.fragment_example, container, false) 
    }
    ```
    
4. **onViewCreated(View view, Bundle savedInstanceState)**:
    
    - Called immediately after `onCreateView()`.
    - Perform view-related tasks such as setting up UI components, listeners, or observers.
5. **onStart()**:
    
    - Called when the fragment becomes visible to the user.
    - The fragment enters the **"Started"** state.
    - Typically used to start animations or other visual elements.
6. **onResume()**:
    
    - Triggered when the fragment is active and interactive (in the foreground).
    - The fragment enters the **"Resumed"** state.
7. **onPause()**:
    
    - Called when the fragment is no longer in focus but still partially visible.
    - Pause tasks like animations or media playback.
8. **onStop()**:
    
    - Called when the fragment is no longer visible to the user.
    - Release resources or save the current state.
9. **onDestroyView()**:
    
    - Called when the fragment's view is destroyed.
    - Clean up resources related to the UI to avoid memory leaks.
10. **onDestroy()**:
    
    - Called when the fragment is destroyed.
    - Perform final cleanup for the fragment's state and components.
11. **onDetach()**:
    
    - Called when the fragment is detached from its host Activity.
    - The fragment's lifecycle is now complete.

---

### **Fragment Lifecycle Flow**

1. **Fragment Created**:
    
    - **onAttach() → onCreate() → onCreateView() → onViewCreated() → onStart() → onResume()**
    - The fragment becomes visible and interactive.
2. **Fragment Interrupted**:
    
    - **onPause() → onStop()**
    - The fragment loses focus or visibility.
3. **Fragment Destroyed**:
    
    - **onDestroyView() → onDestroy() → onDetach()**
    - The fragment and its resources are cleaned up.

---

### **Visual Representation**
<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/fragment-lifecycle.png" alt="">
</figure> 

---

### **Key Notes**

- **onCreateView()** is the key point where the fragment's UI is inflated.
- Use **onViewCreated()** to handle UI setup like listeners or data binding.
- Always release resources like adapters or observers in **onDestroyView()** to prevent memory leaks.
- **onAttach()** and **onDetach()** connect and disconnect the fragment from its host Activity.
- Fragments are often used in dynamic and modular UI designs, especially with navigation components.