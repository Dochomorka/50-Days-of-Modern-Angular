# Day 32: Custom Pipes – Transforming Data in the Template

### **Welcome Back!**
Congratulations on reaching **Day 32**! After mastering programmatic template access yesterday, today we focus on the declarative way to transform data directly within your templates: **Pipes**. By the end of today, you will know how to build and integrate **standalone pipes** while maintaining the clean, **logic-free** component architecture required for enterprise-scale applications.

---

### **Core Concepts**

#### **1. Standalone Pipes and Template Context**
In modern Angular, pipes are **standalone** by default, meaning they are no longer declared in a module. To use a pipe, you must explicitly import it into a component's **template context** via the `imports: []` array. This approach makes dependencies explicit and enables better **tree-shaking** for your production bundles.

#### **2. Supporting the Logic-Free Pattern**
In a clean **Enterprise Architecture**, we strive for UI components that are **logic-free**, receiving data only through inputs. Rather than putting complex transformation logic inside a generic UI component, you should use pipes in the **parent component** (Feature, Pattern, or Layout) to transform the data *before* it is passed down or projected.

#### **3. Pipes as Patterns**
When a pipe contains complex business logic or needs to inject **Services** from the **Core**, it is no longer a generic "UI" element. Instead, it becomes a **Pattern**. These "pattern pipes" can be dropped into various features to provide consistent data formatting—such as currency conversion or status styling—across the application.

---

### **Hands-on Implementation**

#### **Creating a Standalone Currency Pipe**
Let's build a simple pipe that formats a number as a specific currency string, ensuring it is ready for use in any standalone component.

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'appCurrency',
  standalone: true // Mandatory for modern Angular
})
export class CustomCurrencyPipe implements PipeTransform {
  transform(value: number, code: string = 'GBP'): string {
    return new Intl.NumberFormat('en-GB', { 
      style: 'currency', 
      currency: code 
    }).format(value);
  }
}
```

#### **Consuming the Pipe in a Feature**
Import the pipe directly into your feature component to transform data before passing it to a generic UI component.

```typescript
@Component({
  standalone: true,
  imports: [CustomCurrencyPipe, GenericPriceDisplayComponent],
  template: `
    <!-- Data is transformed in the parent feature context -->
    <app-generic-price-display 
      [formattedPrice]="product().price | appCurrency" 
    />
  `
})
export class ProductFeatureComponent {
  product = input.required<Product>();
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Greeting Pipe**: Create a standalone pipe called `WelcomePipe` that appends "Welcome, " to any string. Import it into a component and use it in the template.
2.  **Explicit Context**: Take an existing component and move a built-in pipe (like `JsonPipe` or `UpperCasePipe`) from a module-based setup into the component's **`imports` array**.

#### **Medium Challenges**
1.  **Logic-Free Refactoring**: Identify a **UI component** that performs data formatting (like date manipulation) in its TypeScript logic. Refactor it so the parent component handles the formatting using a pipe before passing the result via an **Input**.
2.  **Multi-Argument Pipe**: Expand your `CustomCurrencyPipe` to accept a second argument for "locale" and verify it updates the formatting accordingly.

#### **Hard Challenges**
1.  **The Pattern Pipe**: Build a pipe that injects an `ExchangeRateService` from the **Core** to convert prices. Explain in your `readme.md` why this pipe must be located in the **`pattern/`** folder rather than the **`ui/`** folder.
2.  **Template-Driven Extraction**: Implement a pattern where you use a complex pipe inside an **`<ng-content>`** slot. Document why this **Template-Driven API** is superior for long-term maintainability compared to passing a fully formatted string through an Input.
3.  **One-Way Analysis**: Write a summary explaining why a pipe in the **`ui/`** folder can never import a service from the **`core/`** and how the **One-Way Dependency Graph** prevents this architectural violation.
