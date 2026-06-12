# Day 3: Standalone Components – The New Standard

### **Welcome Back!**
Congratulations on finishing Day 2! Today, we move into the heart of modern Angular development by mastering **Standalone Components**. Introduced as the new standard, standalone components allow you to build applications without the complexity of **NgModules**, resulting in a leaner, more intuitive architecture.

---

### **Core Concepts**

#### **1. What is a Standalone Component?**
A standalone component is a component that sets the **`standalone: true`** flag in its `@Component` decorator. Unlike legacy components, they do not need to be declared in an `NgModule`. They are self-contained and manage their own dependencies.

#### **2. The Template Context**
In the standalone world, the **Template Context** is explicit. If your component’s template uses another component, directive, or pipe, you must import it directly into the **`imports: []`** array of the `@Component` decorator. This replaces the old "indirect" dependency management provided by modules.

#### **3. Architectural Benefits**
*   **Tree-shaking**: Because dependencies are explicit, the compiler can more easily remove unused code, leading to smaller bundle sizes.
*   **Simplicity**: You no longer have to navigate back and forth between a component and its parent module to add dependencies.
*   **Modularity**: Standalone components are easier to move, reuse, and test in isolation.

---

### **Hands-on Implementation**

#### **Generating a Standalone Component**
Use the Angular CLI to generate a component that is standalone by default:
```bash
ng g c ui/header --standalone
```

#### **Importing Standalone Building Blocks**
In this example, we import a common pipe directly into our component:

```typescript
import { Component } from '@angular/core';
import { TitleCasePipe } from '@angular/common';

@Component({
  selector: 'app-header',
  standalone: true,
  imports: [TitleCasePipe], // Explicitly adding the pipe to the template context
  template: `
    <header>
      <h1>{{ title | titlecase }}</h1>
    </header>
  `
})
export class HeaderComponent {
  title = 'welcome to our app';
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **The Header**: Create a `HeaderComponent` using the CLI and verify that the `standalone: true` flag is present.
2. **Display**: Add a simple navigation title to your header template.

#### **Medium Challenges**
1. **Pipes and Imports**: Use a standalone pipe (like `UpperCasePipe` or `CurrencyPipe`) in your header to format a value. Remember to add it to the `imports` array.
2. **Component Nesting**: Create a simple `UserAvatar` standalone component and import it into your `HeaderComponent` to display next to the title.

#### **Hard Challenges**
1. **Tree-Shaking Analysis**: In your `readme.md`, write a brief explanation of why importing individual standalone pipes is more efficient for "tree-shaking" than importing the entire `CommonModule`.
2. **Architecture Refactor**: If you have an existing project using `NgModules`, pick one small shared component and convert it to a standalone component.

