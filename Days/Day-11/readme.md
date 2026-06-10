# Day 11: Common RxJS Operators

### **Welcome Back!**
You have reached **Day 11**! Yesterday, you learned how to create and subscribe to data streams using **Observables** and **Observers**. Today, we unlock the true power of **RxJS**: **Operators**. Operators are the "logic gates" of your application's data flow. They allow you to transform, filter, and combine streams of data with surgical precision before they ever reach your `subscribe()` block. Mastering these operators is what transforms a standard Angular developer into a professional reactive engineer.

---

### **Core Concepts**

#### **1. The `pipe()` Assembly Line**
Operators are used within the **`pipe()`** method of an Observable. Think of `pipe()` as an assembly line where each operator performs a specific task on the data passing through it. The data enters the top of the pipe, is processed by each operator in sequence, and emerges at the bottom for the subscriber.

#### **2. Transformation Operators**
*   **`map`**: The most common operator. It transforms each emitted value by applying a projection function to it. For example, it can turn an HTTP response object into a specific nested data array.
*   **`switchMap`**: Crucial for Angular HTTP calls. When a new value arrives in the source stream, `switchMap` cancels the previous internal Observable and switches to a new one. This is the primary tool for preventing race conditions in search inputs.

#### **3. Filtering Operators**
*   **`filter`**: Acts as a gatekeeper, only allowing values that meet a specific boolean condition to pass through the stream.
*   **`take(n)`**: Emits only the first `n` values and then automatically completes the stream, helping prevent memory leaks in certain scenarios.

#### **4. Error Handling**
*   **`catchError`**: Intercepts errors that occur in the stream. It allows you to "catch" a failure (like a 404 HTTP error) and return a fallback value or a friendly error stream, ensuring your application doesn't crash.

---

### **Hands-on Implementation**

#### **The Search Logic Pattern**
In modern Angular, we often use operators to clean up user input from a search bar before sending it to a backend service. This pattern demonstrates the power of piping multiple operators together.

```typescript
import { Component, inject } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { debounceTime, distinctUntilChanged, filter, map, switchMap } from 'rxjs';
import { SearchService } from './search.service';

@Component({
  standalone: true,
  imports: [ReactiveFormsModule],
  selector: 'app-search-feature',
  template: `
    <input [formControl]="searchControl" placeholder="Type to search users...">
  `
})
export class SearchFeatureComponent {
  private searchService = inject(SearchService);
  searchControl = new FormControl('');

  // The reactive pipeline
  search$ = this.searchControl.valueChanges.pipe(
    filter(val => !!val && val.length > 2), // Ignore short terms
    debounceTime(300),                      // Wait for user to pause
    distinctUntilChanged(),                 // Ignore if value hasn't changed
    map(val => val!.trim().toLowerCase()),  // Clean the input
    switchMap(term => this.searchService.find(term)) // Cancel old calls
  ).subscribe(results => console.log('Found:', results));
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Doubler**: Create an Observable of numbers (1, 2, 3, 4, 5) and use the **`map`** operator to double each value before logging it to the console.
2.  **Filter the Noise**: Create a stream of both positive and negative numbers. Use the **`filter`** operator to only allow numbers greater than zero to pass through.
3.  **The First Three**: Create a stream that emits numbers every second (using `interval`). Use the **`take(3)`** operator to ensure the stream stops and completes after exactly three emissions.

#### **Medium Challenges**
1.  **Property Picker**: Create a stream of user objects (e.g., `{ id: 1, username: 'angular_dev' }`). Use **`map`** to transform the stream so it only emits the `username` string.
2.  **Conditional Squaring**: Create a stream of numbers from 1 to 20. Pipe them through a **`filter`** to keep only even numbers, and then use **`map`** to square those even numbers.
3.  **Graceful Failure**: Use the `throwError` function to simulate a failing stream. Use **`catchError`** to catch the error and return an `of(['Default Result'])` instead, proving that the subscriber still receives data despite the error.

#### **Hard Challenges**
1.  **Race Condition Simulation**: Build a search simulation where user "keystrokes" trigger simulated API calls (using `timer`). Use **`switchMap`** to prove that if a new keystroke arrives while an old API call is still "pending," the old call is cancelled and its result is ignored.
2.  **Double-Click Prevention**: Research the **`exhaustMap`** operator. Create a "Submit" button click stream and use `exhaustMap` to handle a simulated 2-second saving process. Demonstrate that clicks occurring *while* a save is in progress are completely ignored.
3.  **The Accumulator**: Use the **`scan`** operator to create a "running total" feature. Emmit numbers one by one, and use `scan` to emit the current sum of all numbers received so far (e.g., input `1, 2, 3` results in output `1, 3, 6`).

***

