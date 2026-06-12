# Day 7: Signal-based Inputs and Model Signals

### **Welcome Back!**
**Congratulations** on reaching the one-week milestone of the **50 Days of Modern Angular** challenge! You have successfully mastered the "engine" of reactivity—Signals, Computed Signals, and Effects. Today, we apply that power to component communication. We are moving away from the legacy `@Input()` decorator and embracing **Signal-based inputs**, a more reactive, type-safe, and performant way to pass data between components.

---

### **Core Concepts**

#### **1. The Shift to Signal-based Inputs**
In legacy Angular, `@Input()` was a simple class property. To react to changes, you often had to implement `ngOnChanges` or use setters with side effects. **Signal-based inputs (`input()`)** change this by turning inputs into actual signals. This allows your component to track data changes with **fine-grained reactivity**, triggering updates only where that specific input is used.

#### **2. Required Inputs and Type Safety**
One of the biggest pain points in legacy Angular was ensuring an input actually received data. Modern Signal inputs solve this with the **`.required`** suffix. If you mark an input as required, the TypeScript compiler will ensure the parent component provides it, significantly reducing runtime errors.

#### **3. Model Signals: Two-Way Binding Reimagined**
Historically, two-way data binding required a complex "Banana-in-a-box" syntax (`[(value)]`) paired with a matching `@Output()` emitter. **Model Signals (`model()`)** simplify this into a single primitive. A `model()` signal allows a child component to both read a value and notify the parent of updates automatically, keeping both in perfect synchronization.

#### **4. Architectural Benefits**
Signal-based inputs are a critical building block for **Zoneless Angular**. Because they are reactive by nature, the framework doesn't need to perform expensive "dirty checking" on the entire component tree to see if an input has changed. It simply notifies the specific signals that depend on that input.

---

### **Hands-on Implementation**

Let's build a "User Card" component that receives data via Signal inputs and allows the parent to sync a "Status" using a Model signal.

```typescript
import { Component, input, model } from '@angular/core';

@Component({
  selector: 'app-user-card',
  standalone: true,
  template: `
    <div class="card">
      <!-- 1. Reading a required signal input -->
      <h3>User: {{ username() }}</h3>
      
      <!-- 2. Reading an optional signal input with default -->
      <p>Role: {{ role() }}</p>

      <!-- 3. Model signal for two-way sync -->
      <button (click)="toggleStatus()">
        Status: {{ isActive() ? 'Active' : 'Inactive' }}
      </button>
    </div>
  `
})
export class UserCardComponent {
  // Define a required signal input
  username = input.required<string>();

  // Define an optional signal input with a default value
  role = input<string>('Guest');

  // Define a model signal for two-way data binding
  isActive = model<boolean>(false);

  toggleStatus() {
    // Updating the model signal automatically notifies the parent
    this.isActive.update(current => !current);
  }
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **The Avatar**: Create a `UserAvatarComponent` that takes a `username` as a **required signal input**. Display the first letter of the username in a stylized div.
2. **The Welcome**: Pass a string from a parent component into your avatar component and verify it renders.

#### **Medium Challenges**
1. **The Sync Toggle**: Create a `ToggleComponent` using a **`model()` signal** for a property called `isOn`. 
2. **State Sync**: In your parent component, create a signal called `parentState`. Bind it to the `ToggleComponent` using the `[(isOn)]` syntax and display the `parentState` value in the parent template to prove they are in sync.

#### **Hard Challenges**
1. **Reactivity Analysis**: In your `readme.md`, write a summary explaining why Signal-based inputs are superior for performance in **Zoneless Angular** applications compared to the legacy `@Input()` decorator.
2. **Derived Input Logic**: Create a component that receives a `price` and `taxRate` as signal inputs. Use a **`computed()`** signal inside the component to calculate the `totalPrice` automatically whenever either input changes.
3. **Refactor Exercise**: Take a component from a previous day that used a standard variable and a function to update it. Refactor it to use a `model()` signal and explain how it reduced the amount of "boilerplate" code needed for two-way synchronization.


