# Day 34: Advanced Dependency Injection – Custom Tokens and Factories

### **Welcome Back!**
Congratulations on reaching **Day 34**! You have now mastered the art of combining **Signals** and **RxJS**. Today, we dive deeper into the engine that powers Angular’s modularity: **Dependency Injection (DI)**. While you have been using `inject()` to retrieve services, today we will learn how to provide custom values, configurations, and complex objects using **Injection Tokens** and **Factory Providers**.

---

### **Core Concepts**

#### **1. The Limits of Class-Based Injection**
So far, you have mostly injected **Classes** (like `AuthService`). However, class-based injection doesn't work for **Interfaces** (which disappear after TypeScript compilation) or simple values like **Configuration Objects**. For these scenarios, modern Angular uses **`InjectionToken`**.

#### **2. Injection Tokens (`InjectionToken`)**
An **`InjectionToken`** is a unique object that acts as a lookup key for the DI system. It allows you to inject non-class values with full **Type Safety**. In a clean architecture, global configuration tokens are usually defined in the **`core/`** folder to be accessible by all features.

#### **3. Factory Providers (`useFactory`)**
Sometimes, you need to execute logic to determine what value should be injected. A **Factory Provider** is a function that returns the value. In modern Angular, these factories run within an **Injection Context**, meaning they can use the **`inject()`** function to retrieve other dependencies needed for the initialization.

#### **4. Injector Hierarchy Recall**
Remember that where you provide these tokens matters:
*   **Root Injector**: Registering in `app.config.ts` or `provideCore()` makes the value an **application-wide singleton**.
*   **Environment Injector**: Registering in a lazy-loaded route configuration restricts the value to that specific **Lazy Feature**.
*   **Element Injector**: Registering in a component’s `providers` array creates a **unique instance** for every instance of that component.

---

### **Hands-on Implementation**

#### **Providing an API Configuration Token**
Let's create a custom token for application settings and provide it using a factory in your **`provideCore()`** setup.

```typescript
// core/config/api.config.ts
import { InjectionToken, inject } from '@angular/core';

export interface ApiConfig {
  baseUrl: string;
  timeout: number;
}

export const API_CONFIG = new InjectionToken<ApiConfig>('API_CONFIG');

// core/core.ts
export function provideCore({ routes }: CoreOptions) {
  return [
    provideRouter(routes),
    // Use a factory to provide the token
    {
      provide: API_CONFIG,
      useFactory: () => {
        // You could inject other services here if needed
        return { baseUrl: 'https://api.myorg.com', timeout: 5000 };
      }
    }
  ];
}
```

#### **Injecting the Token**
```typescript
// feature/dashboard/data.service.ts
export class DataService {
  private config = inject(API_CONFIG); // Type-safe configuration
  
  fetchData() {
    console.log(`Connecting to ${this.config.baseUrl}`);
  }
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Greeting Token**: Create a simple `InjectionToken<string>` called `APP_NAME`. Provide it in your `app.config.ts` and inject it into your `MainLayout` component to display the app title in the header.
2.  **Value Provider**: Instead of `useFactory`, use **`useValue`** to provide a constant numeric "Page Size" limit to a feature component.

#### **Medium Challenges**
1.  **The Feature Switch**: Create a factory provider that checks a `localStorage` flag. If the flag is `true`, provide a `MockDataService`; otherwise, provide the real `ProductionDataService`.
2.  **Architectural Token Placement**: Create a configuration token for a specific **Lazy Feature**. Provide it only in the **`providers`** array of that feature's route file and verify that sibling features cannot inject it (resulting in a `NullInjectorError`).

#### **Hard Challenges**
1.  **The Multi-Token Logger**: Research the **`multi: true`** provider option. Create an `ANALYTICS_HANDLER` token and provide three different functions that log to different destinations (Console, Sentry, Analytics). Inject the array of handlers and execute all of them in a single method.
2.  **Environment Initializer**: Use the **`ENVIRONMENT_INITIALIZER`** token in your `provideCore()` function to run a factory that dispatches a "Startup" log or clears a temporary cache.
3.  **Cross-App Isolation**: Following the **"One-Way Dependency Graph"** rules, write a summary explaining why "extracting shared configuration tokens to a library" is required when multiple applications in a workspace need the same token.

