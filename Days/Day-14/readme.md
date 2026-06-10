# Day 14: Route Guards & Resolvers with Functional APIs

### **Welcome Back!**
You have officially reached **Day 14**, marking the end of your second week in the **50 Days of Modern Angular** challenge. After mastering **Lazy Loading** yesterday, today we explore how to protect those routes and pre-fetch data using **Functional APIs**. Modern Angular has moved away from bulky class-based guards in favor of lightweight, composable functions that utilize the **Injection Context** to keep your architecture clean and efficient.

---

### **Core Concepts**

#### **1. The Shift to Functional Guards**
In previous versions of Angular, guards were required to be `@Injectable` classes, which often led to unnecessary boilerplate code. Modern Angular introduces **Functional Guards**, such as **`CanActivateFn`**, allowing you to define route protection as a simple function. These functions are more lightweight and easier to test because they focus strictly on the logic required for navigation.

#### **2. Mastering the Injection Context**
Functional guards work because they run within an **Injection Context**, a special framework state that allows the **`inject()`** function to work outside of a constructor. This enables your guards to inject **Services** or **State Stores** directly into the function body to verify authentication or permissions. If a guard needs to redirect a user, it can return a **`UrlTree`** created by injecting the **`Router`**.

#### **3. Data Resolvers**
**Resolvers** are a specific type of guard used to fetch data **before** a route is activated. This ensures that when a component renders, its required data is already present, preventing "empty" states or flickering UI while waiting for HTTP calls to finish. Like guards, these are now preferred as functions that return an **Observable**, **Promise**, or a direct value.

---

### **Hands-on Implementation**

#### **Creating a Role-Based Guard Factory**
Instead of writing separate guards for every role, you can create a **Factory Function** that returns a tailored guard.

```typescript
// core/auth/auth.guards.ts
export function roleGuard(requiredRole: string): CanActivateFn {
  return () => {
    const authService = inject(AuthService); // Works in injection context
    const router = inject(Router);

    if (authService.hasRole(requiredRole)) {
      return true;
    }

    // Redirect to unauthorized page
    return router.createUrlTree(['/unauthorized']); 
  };
}

// app.routes.ts
export const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./features/admin/admin.routes'),
    canActivate: [roleGuard('admin')] // Composable guard
  }
];
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1. **The Simple Lock**: Create a functional guard called `maintenanceGuard` that checks a boolean signal in a `GlobalSettingsService` and prevents navigation if the app is in "Maintenance Mode".
2. **Logged-In Only**: Implement a basic `authGuard` that returns `true` if a token exists in `localStorage` and redirects to `/login` if it does not.

#### **Medium Challenges**
1. **The Role Factory**: Recreate the `roleGuard` factory shown in the implementation section and apply it to two different lazy-loaded features in your `app.routes.ts`.
2. **Data Prefetcher**: Create a **Functional Resolver** that uses the `ActivatedRouteSnapshot` to get an `:id` parameter and calls a `ProductService` to fetch product details before the page loads.

#### **Hard Challenges**
1. **The Secure Resolver**: Build a "Double-Check" pattern where a resolver first checks if an item exists in the **NgRx State**; if not, it fetches it from the API and, if the API returns a 404, the resolver triggers a redirect to a custom error page.
2. **Architectural Isolation**: Research why placing guard implementation in the **`core/`** folder is an architectural best practice when those guards are used across multiple **Lazy Features**.
3. **Advanced Redirection**: Implement a guard that saves the "Attempted URL" in a service before redirecting to `/login`, so the user can be sent back to their original destination after successfully logging in.

***
