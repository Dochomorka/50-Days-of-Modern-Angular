# Day 9: Injector Hierarchy & Provider Scopes

### **Welcome Back!**
Congratulations on reaching **Day 9**!. Now that you've mastered the `inject()` function, it is time to look at the "underlying reality" of how Angular manages these dependencies. Today, we dive into the **Injector Hierarchy**, the system that determines exactly which instance of a service your components receive. Understanding this hierarchy is the key to building scalable, isolated, and high-performance enterprise applications.

---

### **Core Concepts**

#### **1. The Standard Hierarchy**
Angular organises its injectors in a tree structure. When you request a dependency, Angular looks at the closest injector and searches upwards until it finds a provider or hits the top:
*   **Null Injector:** The absolute top of the tree; it throws an error if a dependency isn't found unless marked as optional.
*   **Platform Injector:** Manages platform-specific services like the `DomSanitizer`.
*   **Root Injector:** This is where **application-wide singletons** live. Services registered here using `providedIn: 'root'` are shared by every consumer in the entire app.

#### **2. The EnvironmentInjector (Lazy Isolation)**
Adding a **lazy-loaded feature** creates its own isolated **EnvironmentInjector**. This is a critical architectural "line of defence". Services provided here are accessible to that feature and its children but are **invisible to sibling features** and the root injector. This enforces the **One-Way Dependency Graph** required for clean enterprise architecture.

#### **3. The ElementInjector (Component Scoping)**
The most fine-grained level is the **ElementInjector**, configured via the `providers: []` array in a `@Component`. This allows you to create a **unique instance** of a service for **every single instance** of that component on the screen. A typical use case for this is the **NgRx ComponentStore**, which needs to manage state independently for each component.

---

### **Hands-on Implementation**

#### **Scoping a Service to a Lazy Feature**
Instead of making everything a global singleton, you can isolate logic within a lazy feature's route configuration:

```typescript
// features/orders/orders.routes.ts
export default [
  {
    path: '',
    component: OrderListComponent,
    providers: [
      OrderService // This service is now isolated to the 'Orders' feature!
    ]
  }
];
```

#### **Using the ElementInjector**
To ensure two components on the same page don't share the same service state, provide the service at the component level:

```typescript
@Component({
  standalone: true,
  selector: 'app-counter',
  providers: [LocalCounterService], // ElementInjector level
  template: `<button (click)="service.inc()">{{ service.count() }}</button>`
})
export class CounterComponent {
  protected service = inject(LocalCounterService);
}
```

---

### **Tiered Challenges**

#### **Easy**
Create a `GlobalSettingsService` and register it using `providedIn: 'root'`. Inject it into two different components to prove they share the same instance of the data.

#### **Medium**
Refactor a "Feature-Specific" service so that it is no longer provided in the root. Instead, register it in a lazy-loaded feature's route configuration. Verify that trying to inject this service into a sibling feature results in a `NullInjectorError`.

#### **Hard**
Build a `TabComponent` that uses a `TabStateService`. Provide the service in the `providers` array of the `TabComponent`. Place three instances of the `TabComponent` on your main page and demonstrate that each one maintains its own independent state without interfering with the others.

***
