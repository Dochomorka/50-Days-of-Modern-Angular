# Day 4: Modern Template Control Flow

### **Welcome Back!**
Congratulations on completing Day 3! You’ve mastered the transition to standalone components. Today, we revolutionize how you write templates. Angular's **Modern Control Flow** replaces legacy structural directives like `*ngIf` and `*ngFor` with a built-in, highly optimized block syntax. This new syntax is not only easier to read but also significantly faster for the framework to process.

---

### **Core Concepts**

#### **1. Built-in Block Syntax**
Modern control flow uses a new syntax starting with the `@` symbol. These blocks are integrated directly into the Angular compiler, meaning they don't require manual imports of `CommonModule` or individual directives. This results in cleaner templates and smaller bundle sizes.

#### **2. Conditional Rendering with `@if`**
The `@if` block provides a declarative way to show or hide content. Unlike the old `*ngIf`, you can now use `@else if` and `@else` directly without needing complex `<ng-template>` or `<ng-container>` wrappers.

#### **3. Optimized Looping with `@for`**
The `@for` block is the modern standard for iterating through collections. It includes two major improvements:
*   **Mandatory `track` expression**: You must provide a unique identifier (like an ID) for each item. This allows Angular to precisely update only the changed elements in the DOM instead of re-rendering the whole list.
*   **Built-in `@empty` block**: You can define exactly what to display if the list is empty, eliminating the need for a separate conditional check.

#### **4. Branching Logic with `@switch`**
For templates with multiple conditions, `@switch` offers a readable alternative to nested if-statements. It works similarly to JavaScript switch statements, using `@case` and `@default` to manage complex UI states.

---

### **Hands-on Implementation**

Let's build a component that displays a list of "Tasks" using these modern blocks.

```typescript
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-task-manager',
  standalone: true,
  template: `
    <div class="container">
      <h2>Task Manager</h2>

      <!-- Conditional Logic -->
      @if (isAdmin()) {
        <p class="badge">Admin Mode Active</p>
      } @else {
        <p class="badge">Viewer Mode</p>
      }

      <!-- Optimized Loop -->
      <ul class="task-list">
        @for (task of tasks(); track task.id) {
          <li>{{ task.name }}</li>
        } @empty {
          <li>No tasks available. Take a break!</li>
        }
      </ul>

      <!-- Switch Logic -->
      @switch (currentStatus()) {
        @case ('loading') { <p>Loading data...</p> }
        @case ('error') { <p class="error">Something went wrong.</p> }
        @default { <p>System ready.</p> }
      }
    </div>
  `
})
export class TaskManagerComponent {
  isAdmin = signal(true);
  currentStatus = signal('idle');
  
  tasks = signal([
    { id: 1, name: 'Master Standalone Components' },
    { id: 2, name: 'Learn Modern Control Flow' },
    { id: 3, name: 'Build Fine-Grained Reactivity' }
  ]);
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **The Toggle**: Create a component with a boolean signal. Use an `@if` block to show a "Welcome" message when true and a "Goodbye" message when false.
2. **Simple List**: Use `@for` to display an array of strings representing your favorite programming languages. Ensure you provide a `track` expression.

#### **Medium Challenges**
1. **Empty States**: Create a "Categories" component. Use a signal that starts as an empty array. Use `@for` and the `@empty` block to display a "No categories found" placeholder. Add a button that populates the array to see the list transition.
2. **Context Variables**: Inside an `@for` loop, use the built-in context variables like `$index`, `$first`, or `$last` to style the first item of your list differently.

#### **Hard Challenges**
1. **The Performance Proof**: Research the mandatory `track` expression. In your `readme.md`, write a summary of how `track` allows Angular's rendering engine to optimize DOM updates compared to the legacy `trackBy` function in `*ngFor`.
2. **Complex Branching**: Build a "Status Dashboard" using `@switch`. Use a signal to toggle between 'online', 'offline', 'away', and 'busy' states, displaying a unique icon and color for each case.

