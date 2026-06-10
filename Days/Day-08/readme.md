# Day 8: Revitalised Dependency Injection

### **Welcome Back!**
Congratulations on reaching **Day 8** and officially entering **Phase 2: Logic & Data**! You have successfully mastered the foundations of modern templates and reactivity; now it is time to look under the hood at the engine that powers Angular: **Dependency Injection (DI)**. Today, we move beyond legacy patterns to master the **`inject()` function**—the cornerstone of clean, functional logic in modern Angular development.

---

### **Core Concepts**

#### **1. The `inject()` Function**
While Angular has long used **constructor-based injection**, the modern standard is the **`inject()` function**. This approach is preferred because it allows you to declare dependencies directly as **class fields**, making your code more readable and removing the need for specific `tsconfig.json` workarounds like `useDefineForClassFields: false` that were often required for standard constructors.

#### **2. Support for Functional APIs**
The introduction of `inject()` has unlocked the ability to use **Functional APIs** across the framework. You are no longer required to author full-blown classes for logic like **route guards** or **HTTP interceptors**; instead, you can write them as lightweight functions that use `inject()` to access necessary services.

#### **3. The Injector Hierarchy**
Angular manages dependencies through a **hierarchical tree** of injectors. Understanding this hierarchy is essential for controlling service instances and application performance:
*   **Null Injector:** The absolute top of the tree, which throws an error if a dependency is not found unless marked as optional.
*   **Platform Injector:** Manages platform-specific services like the `DomSanitizer`.
*   **Root Injector:** Where most services live as **application-wide singletons** when using `providedIn: 'root'`.
*   **EnvironmentInjector:** Created for each lazy-loaded feature, providing **isolated services** that sibling features cannot access.
*   **ElementInjector:** Created at the component or directive level, allowing for a **unique instance** of a service for every component on the screen.

---

### **Hands-on Implementation**

#### **Modern Class Field Injection**
You can now define your dependencies cleanly at the top of your class without a bulky constructor:

```typescript
@Component({
  standalone: true,
  selector: 'app-user-profile',
  template: `<div>{{ userService.user().name }}</div>`
})
export class UserProfileComponent {
  // Direct field-based injection
  protected userService = inject(UserService); 
}
```

#### **Functional Route Guard**
Create a lightweight authentication check using a function instead of an `@Injectable` class:

```typescript
export const authGuard: CanActivateFn = () => {
  const authService = inject(AuthService); // Runs in 'Injection Context'
  const router = inject(Router);

  return authService.isLoggedIn() ? true : router.parseUrl('/login');
};
```

---

### **Tiered Challenges**

#### **Easy**
Refactor a component in your project to delete its `constructor` and use the **`inject()` function** to bring in at least two dependencies (e.g., `HttpClient` and a custom `ThemeService`).

#### **Medium**
Build a **function-based HTTP Interceptor** that uses `inject()` to retrieve a "Base API URL" from an environment configuration service and prepends it to every outgoing request.

#### **Hard**
Research and write a short summary on the **"Injection Context"**. Explain why `inject()` works perfectly during class initialisation or inside a guard, but will throw an error if you attempt to call it inside an asynchronous callback like `setTimeout` or a standard `click` event handler.

***

