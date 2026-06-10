# Day 35: Advanced RxJS – Custom Operators and Schedulers

### **Welcome Back!**
Congratulations on reaching **Day 35**! You have now mastered the complex "brain" of Angular's dependency injection system. Today, we return to the reactive heart of your application to explore **Advanced RxJS**. While you have been using standard operators, today we learn how to build your own **Custom Operators** to encapsulate repetitive logic and how to use **Schedulers** to gain fine-grained control over execution timing.

---

### **Core Concepts**

#### **1. The Power of Custom Operators**
As your application grows, you will often find yourself repeating the same sequence of RxJS operators (e.g., a `filter` followed by a `map`). **Custom Operators** allow you to encapsulate this logic into a single, reusable function, which significantly improves the readability of your streams. This approach follows the principle of **clean architecture** by moving complex transformations out of your components and into focused, testable units.

#### **2. Understanding Schedulers**
By default, RxJS is often synchronous, but **Schedulers** allow you to control the **execution context** of a stream. They determine when a subscription starts and when notifications are delivered, which is critical for performance-sensitive tasks like animations or handling heavy data processing without blocking the main UI thread.

#### **3. Type Safety with Inferred Type Predicates**
With the advent of **TypeScript 5.5**, RxJS became even more powerful through **Inferred Type Predicates**. This feature allows your custom operators to automatically "narrow" types (e.g., removing `null` or `undefined` values) without requiring manual type guards, making your reactive code safer and easier to maintain.

---

### **Hands-on Implementation**

#### **Creating a "Filter Nil" Custom Operator**
One of the most common requirements in reactive Angular apps is filtering out `null` or `undefined` values while maintaining type safety.

```typescript
import { Observable, UnaryFunction, pipe } from 'rxjs';
import { filter, map } from 'rxjs/operators';

/**
 * Custom operator that filters out null/undefined values 
 * and correctly narrows the TypeScript type.
 */
export function filterNil<T>(): UnaryFunction<Observable<T | null | undefined>, Observable<T>> {
  return pipe(
    // Type predicate ensures the output stream only contains T
    filter((value): value is T => value !== null && value !== undefined)
  );
}

// Usage in a service
const data$ = this.http.get<User | null>(url).pipe(
  filterNil(), // Now the stream is guaranteed to be Observable<User>
  map(user => user.name)
);
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Reusable Pipe**: Identify a sequence of two operators you use frequently (e.g., `take(1)` and `map`). Wrap them in a **`pipe()`** function to create a basic custom operator and use it in a component.
2.  **Audit the Stream**: Use the **`tap`** operator inside your custom operator to log values to the console for debugging purposes.

#### **Medium Challenges**
1.  **The Metadata Adder**: Build a custom operator called `addTimestamp`. It should take any incoming value and wrap it in an object that includes a `timestamp` property.
2.  **Local Isolation**: Create a custom operator that handles errors locally using **`catchError`**. Ensure it returns a specific "fallback" value so the main stream never completes unexpectedly.

#### **Hard Challenges**
1.  **The Scheduler Specialist**: Research the **`asyncScheduler`**. Implement a custom operator that uses `observeOn(asyncScheduler)` to delay the delivery of notifications to the next macro-task. Explain in your `readme.md` why this might be useful for avoiding "Expression Changed After It Has Been Checked" errors.
2.  **Advanced Type Narrowing**: Use the new **Inferred Type Predicates** from TypeScript 5.5 to build a custom operator that filters a mixed array of objects based on a specific property, ensuring the resulting stream is perfectly typed.
3.  **Architectural Placement**: Write a technical summary explaining why custom RxJS operators that contain business-specific transformation logic should be stored in the **`pattern/`** folder, while generic utilities belong in **`core/utils/`**.

