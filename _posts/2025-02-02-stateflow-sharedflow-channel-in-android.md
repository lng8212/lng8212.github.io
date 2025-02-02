---
title: Stateflow, SharedFlow and Channel in Android
excerpt: Each of these serves a **different purpose in Android development**. Choosing the right one ensures better performance, correct event handling, and **avoiding UI glitches or memory leaks**.
last_modified_at: 2025-02-02T09:45:06-05:00
header: 
teaser: 
tags:
  - kotlin
  - android
  - flow
  - coroutine
---

### `StateFlow` vs. `SharedFlow` vs. `Channel` – Kotlin Flow Deep Dive

Each of these serves a **different purpose in Android development**. Choosing the right one ensures better performance, correct event handling, and **avoiding UI glitches or memory leaks**.

---

## `StateFlow` – Best for UI State Management

**Use `StateFlow` when you need to hold and share the latest state with multiple collectors**.

### Key Features

✔ **Holds the latest value** (like LiveData).  
✔ **Only emits when the value changes** (no duplicate emissions).  
✔ **All new collectors immediately receive the latest value**.  
✔ **Hot Flow** (keeps running even if no collectors).
✔ **Single active value** (no buffering).

---

### Example: Displaying Network Status in Jetpack Compose

#### Use Case: Show "Connected" or "Disconnected" status in a `Text` view.
```kotlin
@HiltViewModel
class NetworkViewModel @Inject constructor() : ViewModel() {
    private val _networkState = MutableStateFlow("Disconnected")
    val networkState: StateFlow<String> = _networkState  // Expose as immutable

    fun updateNetworkState(isConnected: Boolean) {
        _networkState.value = if (isConnected) "Connected" else "Disconnected"
    }
}

@Composable
fun NetworkStatusScreen(viewModel: NetworkViewModel = hiltViewModel()) {
    val networkState by viewModel.networkState.collectAsState()

    Text(text = "Network Status: $networkState")
}

```

- UI automatically updates when network state changes.  
- Cannot emit duplicate values (e.g., "Connected" → "Connected").  
- Not suitable for UI events (e.g., show Toast on network change).

---

## `SharedFlow` – Best for UI Events and Event Buses

**Use `SharedFlow` when you need to emit events to multiple subscribers.**

### Key Features

✔ Supports **multiple collectors**.  
✔ **Replays past events** (if `replay > 0`).  
✔ Emits **same values multiple times**.  
✔ **Hot Flow** (keeps running even if no collectors).  
✔ **Configurable buffering**.

---

### Example: Navigating to Another Screen on Button Click (Jetpack Compose + Navigation)

#### **Use Case:** When user clicks a button, navigate to a new screen using `SharedFlow`.
```kotlin
@HiltViewModel
class NavigationViewModel @Inject constructor() : ViewModel() {
    private val _navigationEvent = MutableSharedFlow<String>()
    val navigationEvent: SharedFlow<String> = _navigationEvent

    fun navigateTo(destination: String) {
        viewModelScope.launch {
            _navigationEvent.emit(destination)
        }
    }
}

@Composable
fun HomeScreen(navController: NavController, viewModel: NavigationViewModel = hiltViewModel()) {
    val coroutineScope = rememberCoroutineScope()

    LaunchedEffect(Unit) {
        viewModel.navigationEvent.collect { destination ->
            navController.navigate(destination)
        }
    }

    Button(onClick = { viewModel.navigateTo("details_screen") }) {
        Text("Go to Details")
    }
}
```

- Handles one-time UI events properly (like navigation, Snackbars, alerts).  
- Multiple collectors can receive events.  
- If no one is listening, the event is lost(unless `replay > 0`).

---

## `Channel` – Best for Producer-Consumer (Task Queues, Background Jobs)

**Use `Channel` when you need to process tasks one by one (producer-consumer model).**

### Key Features

✔ **One-time event delivery** (each value is consumed only once).  
✔ **Suspends producer if no consumer is available** (if unbuffered).  
✔ **Backpressure handling** (buffers or suspends if full).  
✔ **Supports multiple producers but only one consumer**.

---

### Example: Syncing Data in Background (WorkManager + Channel)

#### **Use Case:** Upload user data to the server in the background.
```kotlin
@HiltViewModel
class SyncViewModel @Inject constructor(private val context: Context) : ViewModel() {
    private val syncChannel = Channel<String>(Channel.UNLIMITED)

    init {
        viewModelScope.launch {
            processSyncRequests()
        }
    }

    fun requestSync(data: String) {
        viewModelScope.launch {
            syncChannel.send(data)  // Adds task to queue
        }
    }

    private suspend fun processSyncRequests() {
        for (data in syncChannel) {
            val workRequest = OneTimeWorkRequestBuilder<SyncWorker>()
                .setInputData(workDataOf("sync_data" to data))
                .build()

            WorkManager.getInstance(context).enqueue(workRequest)
        }
    }
}

class SyncWorker(context: Context, params: WorkerParameters) : CoroutineWorker(context, params) {
    override suspend fun doWork(): Result {
        val data = inputData.getString("sync_data") ?: return Result.failure()
        println("Syncing data: $data")  // Simulate API call
        delay(2000)  // Simulate network delay
        return Result.success()
    }
}

@Composable
fun SyncScreen(viewModel: SyncViewModel = hiltViewModel()) {
    Button(onClick = { viewModel.requestSync("User Profile") }) {
        Text("Sync Data")
    }
}

```

- Queues and processes tasks one by one (good for database writes, network calls).  
- Ensures all tasks are executed (even if app is restarted using `WorkManager`)**.  
- Only one consumer can receive events (not suitable for UI state).
---
## When to Use Which?

| Feature                          | `StateFlow` ✅ | `SharedFlow` ✅          | `Channel` ✅ |
| -------------------------------- | ------------- | ----------------------- | ----------- |
| **Holds latest value**           | ✅ Yes         | ❌ No                    | ❌ No        |
| **Supports multiple collectors** | ✅ Yes         | ✅ Yes                   | ❌ No        |
| **Replays past events**          | ❌ No          | ✅ Yes (if `replay > 0`) | ❌ No        |
| **Emits same value again**       | ❌ No          | ✅ Yes                   | ✅ Yes       |
| **Works well with UI state**     | ✅ Yes         | ❌ No                    | ❌ No        |
| **Best for UI events**           | ❌ No          | ✅ Yes                   | ❌ No        |
| **Best for Background Jobs**     | ❌ No          | ❌ No                    | ✅ Yes       |

---

## Android Use Cases

|**Scenario**|**Best Choice**|
|---|---|
|Updating UI with network status|`StateFlow`|
|Showing Snackbar, Toast, or alerts|`SharedFlow`|
|Navigating to another screen|`SharedFlow`|
|Downloading and processing files in background|`Channel`|
|Syncing user settings across fragments|`StateFlow`|
|Handling user interactions (button clicks)|`SharedFlow`|
|Processing API requests sequentially|`Channel`|

---

## Common Mistakes

|Mistake|Why it's wrong?|What to use instead?|
|---|---|---|
|Using `StateFlow` for button clicks|Doesn't emit same value twice|`SharedFlow`|
|Using `SharedFlow` for UI state|Doesn't store latest value|`StateFlow`|
|Using `Channel` for multiple listeners|Only one consumer gets data|`SharedFlow`|
|Using `StateFlow` for WorkManager|No buffering, no queuing|`Channel`|

---

## Final Thought

If **UI state** → `StateFlow`  
If **UI events** → `SharedFlow`  
If **Background processing** → `Channel`
