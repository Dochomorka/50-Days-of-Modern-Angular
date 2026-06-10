# Day 21: Component Communication – Outputs & Event Emitters

### **Welcome Back!**
Congratulations on reaching **Day 21**! You have now officially completed three full weeks of the **50 Days of Modern Angular** challenge and entered the third decade of your journey toward becoming an **Angular Expert**. Having mastered **Inputs**, **Signals**, and **State Management**, today we focus on the other half of the communication bridge: **Outputs**. By the end of today, you will understand how to build **"logic-free"** UI components that remain perfectly decoupled and universally reusable.

---

### **Core Concepts**

#### **1. The Logic-Free Component Pattern**
A key best practice in **Enterprise Angular Architecture** is to keep the majority of your components **logic-free**. In this model, a component primarily receives data and displays it, while any user interaction is delegated immediately to a parent or a dedicated **Service**. This ensures that business rules are centralized while the UI remains flexible and easy to test.

#### **2. Delegating Responsibility via Outputs**
Generic **UI components**, such as buttons, cards, or menus, should never be bound to a specific application state or service. Instead, they use **Outputs** to notify their parent component that an event—such as a click or a form submission—has occurred. The parent component, which has the necessary context, then decides how to handle that event, often by calling a **headless logic** unit or **state slice** in the **Core**.

#### **3. Modern Functional Outputs**
Following the pattern of modern **Signal-based Inputs**, modern Angular provides the **`output()`** function as a streamlined, type-safe way to define events. This approach is specifically optimized for **Standalone Components** and fits the framework's move toward functional, reactive patterns by removing the boilerplate of legacy decorators.

#### **4. The One-Way Communication Flow**
To maintain a clean **One-Way Dependency Graph**, data should flow **down** into components via **Inputs** and notifications should flow **up** via **Outputs**. This constraint ensures that child components are "black boxes" that do not know about the surrounding application logic, making them universally reusable in any **Feature**, **Pattern**, or **Layout**.

---

### **Hands-on Implementation**

#### **Creating a Generic Action Component**
Let's build a simple `ActionCard` that notifies its parent when a button is clicked, keeping the card itself completely unaware of the application's business logic.

```typescript
import { Component, output, input } from '@angular/core';

@Component({
  standalone: true,
  selector: 'app-action-card',
  template: `
    <div class="p-4 border rounded shadow bg-white">
      <h3 class="font-bold">{{ title() }}</h3>
      <button (click)="notifyParent()" class="mt-2 bg-blue-600 text-white p-2 rounded">
        Execute Action
      </button>
    </div>
  `
})
export class ActionCardComponent {
  // Input for the display data
  title = input.required<string>();

  // Define a modern functional output to notify the parent
  onAction = output<void>();

  notifyParent() {
    // Notify parent without performing any business logic locally
    this.onAction.emit(); 
  }
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Simple Emitter**: Create a `CustomButton` component that uses an **Output** called `onClick`. Verify it triggers a simple console log in the parent component when pressed.
2.  **Passing Data Up**: Update your emitter to send a string value (e.g., a "Button ID") back to the parent and display that ID in the parent's template.

#### **Medium Challenges**
1.  **The Form Bridge**: Build a `UserForm` component that captures a name and email. Use an **Output** to emit an object containing this data only when the "Submit" button is clicked.
2.  **Logic-Free Refactoring**: Take a component that currently injects a **Service** and refactor it so that it instead emits an event, letting the parent component handle the service call.

#### **Hard Challenges**
1.  **Reusability Technical Summary**: Research and write a short summary in your `readme.md` explaining why components that communicate exclusively through **Inputs** and **Outputs** are considered 3–5 times more valuable for **Enterprise Architecture** than components coupled to direct service injections.
2.  **Complex Payload Validation**: Implement a component that emits a custom **TypeScript Interface**. Ensure the parent correctly types the incoming `$event` and uses it to update a **SignalStore** or trigger an **NgRx Action**.

