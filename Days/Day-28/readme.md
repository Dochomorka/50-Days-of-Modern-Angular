# Day 28: State Management – Custom Store Features

### **Welcome Back!**
Congratulations on reaching **Day 28**! Yesterday, you learned how to manage large collections with **SignalStore Entities**. Today, we unlock the final level of **NgRx SignalStore** mastery: **Custom Store Features**. The true power of the SignalStore lies in its extensensibility; it allows you to extract repetitive logic—like loading states, logging, or persistence—into reusable "plugins" that can be dropped into any store in your application.

---

### **Core Concepts**

#### **1. The Extensible Architecture**
The **NgRx SignalStore** is designed to be a versatile solution for effective state management by providing an opinionated yet modular design. Instead of building every store from scratch, you can use the **`signalStoreFeature`** function to create reusable building blocks. These features can add state, computed signals, and methods to any store they are composed into.

#### **2. Feature Factories**
Custom features are typically authored as **factory functions**. This allows you to pass configuration (like a feature name or initial state) into the feature when it is created. In a clean architecture, these shared features are often stored in the **`core/`** folder because they provide headless logic used by multiple features.

#### **3. Composable Logic**
By creating custom features, you move away from "fat" stores and toward **composable logic**. For example, you might create a `withCallState()` feature that manages `loading` and `error` states. You can then add this to a `ProductStore`, an `OrderStore`, and a `UserStore` without rewriting a single line of state logic.

---

### **Hands-on Implementation**

#### **Creating a Reusable "Call State" Feature**
This common pattern manages the status of asynchronous operations (e.g., 'init', 'loading', 'loaded', or an error message).

```typescript
import { signalStoreFeature, withState, withComputed } from '@ngrx/signals';
import { computed } from '@angular/core';

export type CallState = 'init' | 'loading' | 'loaded' | { error: string };

export function withCallState() {
  return signalStoreFeature(
    withState<{ callState: CallState }>({ callState: 'init' }),
    withComputed(({ callState }) => ({
      loading: computed(() => callState() === 'loading'),
      error: computed(() => {
        const state = callState();
        return typeof state === 'object' ? state.error : null;
      }),
    }))
  );
}

// Consuming the custom feature in a store
export const ProductStore = signalStore(
  { providedIn: 'root' },
  withCallState(), // Adds callState, loading, and error automatically!
  withState({ products: [] })
);
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Identity Feature**: Create a simple custom feature called `withLastUpdated` that adds a single state property `lastUpdated` (a timestamp) to a store.
2.  **Feature Integration**: Create a `TodoStore` and compose it using your new `withLastUpdated` feature. Verify that the `lastUpdated` signal is available in your component.

#### **Medium Challenges**
1.  **The Logger Plugin**: Build a custom feature that uses a **Lifecycle Hook** (like `onInit`) to log a message to the console whenever the store is initialized.
2.  **Status Factory**: Refactor the `withCallState` example to accept an optional initial value (e.g., starting in 'loading' instead of 'init') via a factory argument.

#### **Hard Challenges**
1.  **The Persistence Engine**: Research the `effect` function in Angular. Build a custom feature called `withLocalStorage` that accepts a key. Use an `effect` within the feature to automatically save the store's state to `localStorage` every time it changes.
2.  **Architectural Sharing**: Following the **"Extract One Level Up"** rule, move your custom features into a `core/state/features/` folder. Implement a `UserStore` and a `SettingsStore` in two different lazy features that both consume these shared core features.
3.  **Entity Integration**: Create a custom feature that specifically extends a store using **Entities**. Build a `withSelectedEntity` feature that adds a `selectedId` property and a `selectedEntity` computed signal that finds the item in the `entityMap`.

