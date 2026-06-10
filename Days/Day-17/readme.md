# Day 17: Intro to NgRx SignalStore

### **Welcome Back!**
Congratulations on reaching **Day 17** of the **50 Days of Modern Angular** challenge. Yesterday, we unlocked the peak of performance with **Zoneless Change Detection**, and today we are going to master the state management solution built specifically for this new era: the **NgRx SignalStore**. You are moving away from ad-hoc services toward a robust, declarative way to manage your application's state with native **Signals** support.

---

### **Core Concepts**

#### **1. What is SignalStore?**
The **NgRx SignalStore** is a fully-featured state management solution designed to be simple, flexible, and extensible. It establishes a versatile solution for effective state management in Angular by providing an opinionated yet modular design. Unlike legacy state libraries, it returns an **injectable service** that you can provide and inject exactly where you need it.

#### **2. Creating a Store with Features**
You create a store using the **`signalStore()`** function, which accepts a sequence of **store features**. 
*   **`withState`**: This feature adds state slices to your store and automatically creates corresponding signals for each property.
*   **Deep Reactivity**: One of the most powerful aspects is that **DeepSignals** are generated lazily on demand for nested state properties.
*   **`withComputed`**: This allows you to add **computed signals** to the store, which are automatically updated when their underlying state dependencies change.
*   **`withMethods`**: This feature allows you to define the logic used to update the store's state or perform other side effects.

#### **3. Providing and Injecting**
By default, a **SignalStore** is not registered with any injector. You must include it in a **`providers`** array at the component level to tie it to a specific lifecycle, or set **`providedIn: 'root'`** to make it an application-wide singleton.

---

### **Hands-on Implementation**

#### **Building a Simple Task Store**
In this example, we define a store to manage a simple task list with a computed count of completed items.

```typescript
import { signalStore, withState, withComputed, withMethods, patchState } from '@ngrx/signals';
import { computed } from '@angular/core';

export const TaskStore = signalStore(
  { providedIn: 'root' }, // Global registration
  withState({ 
    tasks: [{ id: 1, title: 'Learn SignalStore', completed: false }],
    loading: false 
  }),
  withComputed(({ tasks }) => ({
    completedCount: computed(() => tasks().filter(t => t.completed).length)
  })),
  withMethods((store) => ({
    addTask(title: string) {
      patchState(store, (state) => ({
        tasks: [...state.tasks, { id: Date.now(), title, completed: false }]
      }));
    }
  }))
);
```

#### **Using the Store in a Component**
Once defined, you can inject the store and access its reactive state directly in your template.

```typescript
@Component({
  standalone: true,
  template: `
    <h2>Tasks: {{ store.completedCount() }} completed</h2>
    @for (task of store.tasks(); track task.id) {
      <div>{{ task.title }}</div>
    }
  `
})
export class TaskListComponent {
  readonly store = inject(TaskStore); // Injecting our reactive store
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **The State Builder**: Create a **SignalStore** using **`withState`** that holds a single `user` object with `name` and `email` properties. 
2. **Local Provider**: Register your new store in the **`providers`** array of a standalone component instead of the root injector and verify it works.

#### **Medium Challenges**
1. **Derived Logic**: Add a **`withComputed`** feature to a store that calculates a "status" string (e.g., 'Active' vs 'Idle') based on a boolean `isActive` state signal.
2. **State Patching**: Implement a **`withMethods`** feature that includes a function to update the user's email using the **`patchState`** utility.

#### **Hard Challenges**
1. **The Asynchronous Store**: Research the **`rxMethod`** from the `@ngrx/signals/rxjs-interop` plugin. Create a store method that takes a search term and simulates an API call using **RxJS** operators like `pipe`, `debounceTime`, and `switchMap`.
2. **Architectural Isolation**: Research and write a technical summary on why providing a **SignalStore** at the **ElementInjector** (component) level is preferred for features that need isolated state instances, such as a reusable **Tab** or **Dialog** component.

***

