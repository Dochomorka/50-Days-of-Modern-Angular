# Day 6: Mastering Computed Signals and Effects

### **Welcome Back!**
Congratulations on completing Day 5! You’ve mastered the basics of writable signals, the foundation of modern Angular reactivity. Today, we take that reactivity to the next level by exploring **Computed Signals** and **Effects**. These tools allow you to build complex, dependent logic that stays perfectly in sync with your application state while maintaining peak performance.

---

### **Core Concepts**

#### **1. Computed Signals: Derived State**
A **Computed Signal** is a read-only signal that derives its value from other signals. You create one using the `computed()` function. 
*   **Automatic Tracking**: You don't have to tell Angular which signals to watch; it automatically tracks any signal read inside the `computed` function.
*   **Memoization**: Computed signals are highly efficient. They only recalculate their value when one of their dependency signals changes. If the value hasn't changed, they return the cached result.
*   **Lazy Evaluation**: The calculation only runs when the computed signal is actually read.

#### **2. Effects: Reactive Side Effects**
An **Effect** is an operation that runs whenever one or more signal values change. Use an `effect()` when you need to perform logic that doesn't return a value but interacts with the "outside world."
*   **Common Use Cases**: Logging data to the console, syncing with `localStorage`, or manually manipulating a 3rd-party library (like a chart).
*   **Injection Context**: Effects must be created within an **Injection Context**, such as a component constructor or as a field initializer. This is because they are tied to the lifecycle of the consumer (e.g., the component) and are automatically cleaned up when that consumer is destroyed.

---

### **Hands-on Implementation**

Let's build a "Shopping Cart" summary that calculates a total price and logs every change.

```typescript
import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-cart-summary',
  standalone: true,
  template: `
    <div class="cart">
      <h2>Shopping Cart</h2>
      <p>Item: Wireless Mouse ($25)</p>
      
      <label>Quantity:</label>
      <button (click)="changeQty(-1)">-</button>
      <span> {{ quantity() }} </span>
      <button (click)="changeQty(1)">+</button>

      <hr />
      <h3>Total Price: {{ totalPrice() | currency }}</h3>
      <p>Status: {{ status() }}</p>
    </div>
  `
})
export class CartSummaryComponent {
  // 1. Writable signals
  quantity = signal(1);
  price = signal(25);

  // 2. Computed signal (derived state)
  totalPrice = computed(() => this.quantity() * this.price());

  // 3. Another computed signal based on the first computed signal
  status = computed(() => this.totalPrice() > 100 ? 'Free Shipping Eligible' : 'Add more items for free shipping');

  constructor() {
    // 4. Effect (side effect)
    effect(() => {
      console.log(`The total price is now: ${this.totalPrice()}`);
    });
  }

  changeQty(amount: number) {
    this.quantity.update(q => Math.max(0, q + amount));
  }
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **Full Name**: Create a component with `firstName` and `lastName` writable signals. Create a `fullName` computed signal that joins them and display it in the template.
2. **Double Count**: Create a `count` signal and a `doubleCount` computed signal. Add a button to increment the count and verify both update.

#### **Medium Challenges**
1. **Search Filter**: Create a `searchTerm` signal and a `list` signal (an array of strings). Create a `filteredList` computed signal that returns only the items containing the search term. 
2. **Effect Sync**: Use an `effect` to log a warning message to the console only when a `count` signal exceeds 10.

#### **Hard Challenges**
1. **LocalStorage Logger**: Implement a component that tracks a `theme` signal ('light' or 'dark'). Use an `effect` to save the current theme to `localStorage` every time it changes.
2. **Performance Summary**: In your `readme.md`, write a short technical summary explaining the benefit of computed signals being "lazy and memoized" versus calling a standard TypeScript getter or function in an Angular template. Focus on how this helps Angular avoid unnecessary work during change detection.

