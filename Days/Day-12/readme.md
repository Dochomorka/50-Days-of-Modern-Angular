# Day 12: RxJS & Signals Interop – `toSignal` and `toObservable`

### **Welcome Back!**
Congratulations on reaching **Day 12**! You have already mastered the foundations of **Signals** for fine-grained state management and **RxJS** for powerful asynchronous streams. Today, we bring these two worlds together. In modern Angular development, you don't have to choose between them; instead, you learn to bridge them using **interop functions** to create a seamless, reactive architecture.

---

### **Core Concepts**

#### **1. Why Interop?**
While **Signals** are the new standard for state in templates, **RxJS** remains the superior tool for complex asynchronous logic, such as handling **HTTP requests**, **WebSockets**, or **time-based operations** like debouncing. Interop allows you to use the best tool for the job: process data with **RxJS operators** and then expose it to your template as a **Signal** for maximum performance.

#### **2. `toSignal()`: From Stream to State**
The **`toSignal()`** function tracks an **Observable** and returns a **Signal** that always stays in sync with the latest emitted value. 
*   **Initial Value:** Since Observables might not emit immediately, you often provide an `initialValue`.
*   **Injection Context:** Like effects, `toSignal` must typically be called during component construction or as a class field initialiser.

#### **3. `toObservable()`: From State to Stream**
The **`toObservable()`** function does the opposite: it creates an **Observable** that emits whenever a **Signal's** value changes. This is incredibly useful when you want to take a reactive state value and pipe it through **RxJS operators** like `debounceTime`, `filter`, or `switchMap`.

---

### **Hands-on Implementation**

#### **Converting an API Stream to a Signal**
Instead of using the `async` pipe, you can now convert your HTTP calls directly into signals for cleaner templates.

```typescript
import { Component, inject } from '@angular/core';
import { toSignal } from '@angular/core/rxjs-interop';
import { UserService } from './user.service';

@Component({
  standalone: true,
  template: `
    @if (user()) {
      <p>Welcome, {{ user()?.name }}!</p>
    }
  `
})
export class UserProfileComponent {
  private userService = inject(UserService);

  // toSignal creates a read-only signal from the Observable
  user = toSignal(this.userService.getCurrentUser()); 
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Delayed Counter:** Create an **Observable** using `interval(1000)` and convert it to a **Signal** using `toSignal`. Display the count in your template.
2.  **Default Values:** Create a stream that emits a string after 3 seconds. Use `toSignal` with an `initialValue` of "Loading..." to ensure the template always has a value to display.

#### **Medium Challenges**
1.  **Reactive Search Bridge:** Create a `searchTerm` **Signal**. Convert it to an **Observable** using `toObservable`, pipe it through a `debounceTime(500)` operator, and log the results to the console.
2.  **Stream Combiner:** Take two separate **Observables** (e.g., a list of items and a filter string) and use `toSignal` on each. Create a **`computed()`** signal that filters the list based on the other signal's value.

#### **Hard Challenges**
1.  **The Auto-Saver:** Build a form where every keystroke updates a `formData` **Signal**. Use `toObservable` to watch that signal, apply `distinctUntilChanged`, and then use `switchMap` to trigger a simulated "Save API" call.
2.  **Manual Injector Context:** Research the `manualCleanup` and `injector` options in `toSignal`. Create a service method that uses `toSignal` *outside* of a constructor by manually passing an `Injector` instance.
3.  **Handling Errors in Interop:** Build a stream that emits an error. Demonstrate how to use the `rejectErrors` option in `toSignal` or provide a fallback value using `catchError` before the conversion to keep your Signal state stable.

***
