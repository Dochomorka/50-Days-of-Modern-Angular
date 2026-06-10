# Day 37: Advanced Routing – Resolvers, Guards, and Component Input Binding

### **Welcome Back!**
Congratulations on reaching **Day 37**! Yesterday, you mastered the model-first approach of signal-based forms, and today we are elevating your navigation logic to match that modern standard. We are moving beyond basic route definitions to explore **Advanced Routing**, focusing on how to secure paths, pre-fetch data, and simplify how your components consume route information using **Component Input Binding**.

---

### **Core Concepts**

#### **1. Functional Guards**
Modern Angular has moved toward **function-based** building blocks for route guards and interceptors, replacing the older, more verbose class-based decorators. These lightweight functions run within an **injection context**, allowing them to use the **`inject()`** function to access services like an `AuthService` directly without the need for a constructor.

#### **2. Component Input Binding**
One of the most powerful modern routing features is the ability to map **route parameters**, **query parameters**, and **data** directly to component **Inputs**. By enabling **`withComponentInputBinding()`** in your router configuration, you can eliminate the need to manually inject `ActivatedRoute` into your components, keeping them cleaner and more **logic-free**.

#### **3. Resolvers and Headless Logic**
In a clean **Enterprise Architecture**, logic that fetches data before a route is activated is considered **headless logic**. These resolvers are provided in the **Core** or at the **Feature** level to ensure that the necessary state is ready before the user ever sees the template.

---

### **Hands-on Implementation**

#### **Configuring Modern Routing**
Update your global `provideCore()` function to enable advanced input mapping and blocking navigation until initialization is complete.

```typescript
// core/core.ts
export function provideCore({ routes }: CoreOptions) {
  return [
    provideRouter(
      routes,
      // Enables route params to be bound directly to input() signals
      withComponentInputBinding(), 
      withEnabledBlockingInitialNavigation()
    ),
    // ... other core providers
  ];
}
```

#### **Implementing a Functional Auth Guard**
Create a lightweight guard that checks authentication state using the modern `inject()` pattern.

```typescript
// core/auth/auth.guard.ts
import { inject } from '@angular/core';
import { Router, CanActivateFn } from '@angular/router';
import { AuthService } from './auth.service';

export const authGuard: CanActivateFn = () => {
  const authService = inject(AuthService); // Injection within the function context
  const router = inject(Router);

  if (authService.isAuthenticated()) {
    return true;
  }
  
  // Redirect to login if unauthorized
  return router.createUrlTree(['/login']); 
};
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The ID Input**: Create a `UserDetail` component and use a **Signal-based input** to capture a `userId` directly from a route parameter (e.g., `/user/:userId`).
2.  **Title Mapping**: Use your router configuration in `app.routes.ts` to pass a static `title` string in the route's **`data`** object and verify it appears in your component as an Input.

#### **Medium Challenges**
1.  **The Protected Feature**: Implement a **functional guard** that prevents access to an "Admin" feature unless a specific role is found in the `AuthService`.
2.  **Global Redirect**: Update your `app.routes.ts` to include a **fallback route** (`**`) that redirects users to your `home` lazy feature if no path matches.

#### **Hard Challenges**
1.  **The State Resolver**: Build a resolver that fetches a "Product" from a **SignalStore** or service in the **Core**. Use **Component Input Binding** to receive this resolved product directly in your feature component.
2.  **Architectural Isolation**: Write a technical summary in your `readme.md` explaining why extracting shared routing logic into the **Core** is necessary when multiple lazy features need to access the same protected data or guards.
3.  **Zoneless Navigation Proof**: Research and document how **blocking initial navigation** and **component input binding** contribute to a smoother user experience in a **Zoneless Angular** environment by ensuring data is ready before the first change detection cycle.

