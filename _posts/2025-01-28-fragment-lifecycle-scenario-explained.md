---
title: Fragment Lifecycle Scenarios Explained
excerpt: Examples with their corresponding lifecycle charts for clarity.
last_modified_at: 2025-01-28T09:45:06-05:00
header: 
teaser: 
tags:
  - android
  - lifecycle
---
### 1. **Fragment Lifecycle with `FragmentManager`**

Below are the examples with their corresponding lifecycle **charts** for clarity.

---

#### a. **Add Fragment Without `addToBackStack`**

```kotlin
supportFragmentManager
	.beginTransaction()
	.add(R.id.fragment_container,FragmentA())
	.commit()
```

**Scenario**: You add `FragmentA` without saving it to the back stack.

|**FragmentA Lifecycle**|**State**|
|---|---|
|`onAttach()`|Fragment is attached to the host.|
|`onCreate()`|Fragment is initialized.|
|`onCreateView()`|Fragment's UI is created.|
|`onStart()`|Fragment becomes visible.|
|`onResume()`|Fragment is active.|

**Behavior**:

- Adding a new fragment (e.g., `FragmentB`) destroys `FragmentA`'s view but keeps it **attached** in memory.
- No back stack. Pressing the back button **exits the app**.

---

#### b. **Add Fragment With `addToBackStack`**

```kotlin
supportFragmentManager
	.beginTransaction()     
	.add(R.id.fragment_container, FragmentA())     
	.addToBackStack(null)     
	.commit()
```

**Scenario**: `FragmentA` is added and saved to the back stack.

|**FragmentA Lifecycle**|**State**|
|---|---|
|`onAttach()`|Fragment is attached.|
|`onCreate()`|Fragment is initialized.|
|`onCreateView()`|Fragment's UI is created.|
|`onStart()`|Fragment becomes visible.|
|`onResume()`|Fragment is active.|

**Behavior**:

- Adding a new fragment (e.g., `FragmentB`):
    - `FragmentA` calls `onPause` and `onStop` but remains in memory.
- Pressing back restores `FragmentA` from memory.

---

#### c. **Replace Fragment Without `addToBackStack`**

```kotlin
supportFragmentManager
	.beginTransaction()     
	.replace(R.id.fragment_container, FragmentA())     
	.commit()
```

**Scenario**: Replace any existing fragment with `FragmentA`. Old fragments are destroyed.

|**Old Fragment Lifecycle**|**New Fragment (FragmentA)**|
|---|---|
|`onPause()`|`onAttach()`|
|`onStop()`|`onCreate()`|
|`onDestroyView()`|`onCreateView()`|
|`onDestroy()`|`onStart()`|
|`onDetach()`|`onResume()`|

**Behavior**:

- Old fragments are **fully destroyed**.
- No back stack. Pressing the back button exits the app.

---

#### d. **Replace Fragment With `addToBackStack`**

```kotlin
supportFragmentManager
	.beginTransaction()     
	.replace(R.id.fragment_container, FragmentA())     
	.addToBackStack(null)     
	.commit()
```

**Scenario**: Replace the old fragment with `FragmentA` and save the old one to the back stack.

|**Old Fragment Lifecycle**|**New Fragment (FragmentA)**|
|---|---|
|`onPause()`|`onAttach()`|
|`onStop()`|`onCreate()`|
|`onDestroyView()`|`onCreateView()`|
|_(Retained in memory)_|`onStart()`|
||`onResume()`|

**Behavior**:

- Old fragments are **paused and stopped**, but not destroyed.
- Pressing back removes `FragmentA` and restores the old fragment.

---

### 2. **NavController with NavGraph**

---

#### a. **Navigate to a New Fragment**

```kotlin
findNavController().navigate(R.id.action_fragmentA_to_fragmentB)
```

**Scenario**: `FragmentA` navigates to `FragmentB` via the NavController.

|**FragmentA Lifecycle**|**FragmentB Lifecycle**|
|---|---|
|`onPause()`|`onAttach()`|
|`onStop()`|`onCreate()`|
|`onDestroyView()`|`onCreateView()`|
|_(Removed from memory)_|`onStart()`|
||`onResume()`|

**Behavior**:

- **FragmentA** is removed from memory entirely.
- **FragmentB** starts its lifecycle from `onAttach()`.

---

#### b. **Back Navigation**

When the back button is pressed:

- **FragmentB Lifecycle**:
    - `onPause`, `onStop`, `onDestroyView`, `onDestroy`, and `onDetach`.
- **FragmentA Lifecycle**:
    - Recreated from `onCreateView`, followed by `onStart` and `onResume`.

|**FragmentB Lifecycle**|**FragmentA Lifecycle**|
|---|---|
|`onPause()`|`onCreateView()`|
|`onStop()`|`onStart()`|
|`onDestroyView()`|`onResume()`|
|`onDestroy()`||
|`onDetach()`||

---

#### c. **Removing a Fragment**

If `FragmentB` is removed manually:

- **FragmentB Lifecycle**:
    - Fully destroyed: `onPause`, `onStop`, `onDestroyView`, `onDestroy`, and `onDetach`.
- **FragmentA Lifecycle**:
    - Brought back into the foreground: `onCreateView`, `onStart`, and `onResume`.

---

### **Comparison of NavController vs. FragmentManager**

|**Feature**|**FragmentManager**|**NavController**|
|---|---|---|
|**Old Fragment Behavior**|Retained in memory with `addToBackStack`.|Removed from memory entirely.|
|**Back Stack Handling**|Manual with `addToBackStack`.|Automatic back stack handling.|
|**Lifecycle Resumption**|Old fragment resumes lifecycle.|Old fragment is recreated.|
|**Ease of Use**|More control, more code.|Simplified navigation.|