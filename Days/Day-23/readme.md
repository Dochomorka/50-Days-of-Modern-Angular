# Day 23: Directives – Customising DOM Behaviour

### **Welcome Back!**
Welcome to **Day 23** of your journey toward mastering modern Angular! You have already perfected component communication and architecture; today, we focus on the framework's primary tool for extending the behaviour of existing elements: **Directives**. By the end of today, you will know how to build reusable, **standalone** directives that bring your templates to life without cluttering your component logic.

---

### **Core Concepts**

#### **1. What are Directives?**
**Directives** are classes that allow you to attach custom behaviour to elements in the DOM or even transform its structure. In modern Angular, directives are **standalone** by default, meaning they are no longer declared in a parent module but are instead imported directly into the **template context** of the component that needs them.

#### **2. Types of Directives**
*   **Attribute Directives:** These modify the appearance or behaviour of an existing element, component, or another directive. Examples include handling hover states, managing **focus**, or applying **Tailwind CSS** classes dynamically.
*   **Structural Directives:** These change the DOM layout by adding or removing elements. While legacy directives like `*ngIf` exist, modern Angular uses the **built-in control flow** (`@if`, `@for`) for these tasks.

#### **3. Directives in UI Architecture**
In a clean **Enterprise Architecture**, generic reusable directives are stored in the **`ui/`** folder. These directives, such as a `drag` or `drop` utility, should be **logic-free**, meaning they do not depend on business state from the **core**. Instead, they communicate exclusively through **inputs** and **outputs**, making them universally reusable across any **feature** or **pattern**.

---

### **Hands-on Implementation**

#### **Creating a Hover-Highlight Directive**
Let's build a simple directive that changes an element's style when the user hovers over it, using the modern **`inject()`** function.

```typescript
import { Directive, ElementRef, HostListener, inject, input } from '@angular/core';

@Directive({
  standalone: true,
  selector: '[appHighlight]' // Applied as an attribute
})
export class HighlightDirective {
  private el = inject(ElementRef);
  
  // Signal-based input for the highlight color
  color = input<string>('yellow'); 

  @HostListener('mouseenter') onMouseEnter() {
    this.el.nativeElement.style.backgroundColor = this.color();
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.el.nativeElement.style.backgroundColor = '';
  }
}
```

To use this, simply add it to your component's **`imports`** array and apply the `appHighlight` attribute to any tag.

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Shadow Modifier**: Create a directive that automatically applies a specific **Tailwind shadow class** to an element when it is clicked.
2.  **Instant Uppercase**: Implement a directive that immediately transforms the text content of its host element to uppercase upon initialisation.

#### **Medium Challenges**
1.  **The Tooltip Logger**: Build a directive that listens for a mouse hover and logs a custom message (passed via a **Signal input**) to the developer console.
2.  **Auto-Focus Utility**: Create a directive that uses **`ElementRef`** to automatically focus an input field as soon as the component renders on the screen.

#### **Hard Challenges**
1.  **Architectural Isolation**: Write a brief technical summary in your `readme.md` explaining why directives in the **`ui/`** folder must never inject services from the **`core/`** folder.
2.  **ElementInjector Specialist**: Build a directive that uses the **`providers`** array to create a **unique instance** of a `LocalStateService` specifically for the element it is attached to.
3.  **Dynamic Class Stylist**: Research **`Renderer2`**; implement a directive that accepts an array of **Tailwind utility classes** as an input and applies them dynamically to the element to avoid direct DOM manipulation.

