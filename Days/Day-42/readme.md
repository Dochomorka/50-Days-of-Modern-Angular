# Day 42: Security – Protecting the Enterprise App

### **Welcome Back!**
Congratulations on reaching **Day 42**! After making your application global and high-performing, we now turn our focus to **Security**. In an enterprise environment, protecting data is paramount. Today, we will learn how to leverage modern Angular features to implement robust security patterns, focusing on the critical distinction between frontend user experience and backend data protection.

---

### **Core Concepts**

#### **1. The Responsibility Split**
It is vital to remember that true security is the sole responsibility of the **Backend**. Since frontend code can always be "hacked" or modified using developer tools, frontend security measures are primarily for **User Experience (UX)**—ensuring users don't see buttons or routes they aren't permitted to use.

#### **2. Interceptors as the First Line of Defense**
In modern Angular, we use **functional interceptors** to handle outgoing requests. These interceptors are provided globally in the **Core** and are responsible for attaching authentication tokens (like JWTs) to every HTTP header, ensuring the backend can verify the user's identity for every transaction.

#### **3. Security-Focused Core Logic**
Sensitive information like the **Authentication Token** and the **User Entity** (containing roles) should be managed within the **Core**. This ensures the state is available from the moment the application bootstraps and can be used by guards and layouts before any lazy features are loaded.

#### **4. Guards for Feature Access**
As we explored on Day 37, **Functional Guards** use the auth state in the core to determine if a user can navigate to a specific **Lazy Feature**. This prevents unauthorized users from accidentally loading heavy feature bundles or seeing protected UI elements.

---

### **Hands-on Implementation**

#### **Implementing a Functional Auth Interceptor**
Let's build a modern interceptor that automatically attaches an auth token from a service in the **Core** to all outgoing requests.

```typescript
// core/auth/auth.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { AuthService } from './auth.service';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authService = inject(AuthService);
  const token = authService.getToken();

  // Clone the request to add the new header
  if (token) {
    const authReq = req.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`
      }
    });
    return next(authReq);
  }

  return next(req);
};
```

#### **Registering the Interceptor in Core**
Update your **`provideCore()`** function to include the interceptor.

```typescript
// core/core.ts
export function provideCore({ routes }: CoreOptions) {
  return [
    provideHttpClient(
      withInterceptors([authInterceptor]) // Register functional interceptor
    ),
    // ... other providers
  ];
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Header Check**: Open the **Network tab** in your browser's dev tools. Verify that your API requests now include the `Authorization` header with your mock token.
2.  **User Info Display**: In your `MainLayout`, inject the `AuthService` and display the current user's name or avatar, ensuring it is visible before any lazy feature loads.

#### **Medium Challenges**
1.  **Role-Based UI**: Create a **computed signal** in a component that checks if the current user has an `'admin'` role. Use this signal with an **`@if`** block to hide or show a "Delete" button.
2.  **Auth Guard Integration**: Update your `authGuard` from Day 37 to check for a specific role retrieved from the `AuthService` before allowing access to a route.

#### **Hard Challenges**
1.  **Global Error Handling**: Implement a second interceptor that listens for **401 Unauthorized** errors. If detected, have it trigger a logout process in the `AuthService` and redirect the user to the login page.
2.  **Security Architecture Summary**: Write a short technical summary in your `readme.md` explaining why frontend guards are considered **UX features** rather than "real" security, and why the backend must always perform the final validation.

