# Day 43: Advanced Interceptors – Logging, Caching, and Retries

### **Welcome Back!**
Congratulations on reaching **Day 43**! Yesterday, you secured your application using **functional interceptors** to handle authentication tokens. Today, we expand that logic to handle other critical "infrastructure" concerns: **Logging**, **Caching**, and **Retries**. These patterns ensure your application is not only secure but also resilient and performant by managing global request behaviour in a single, **headless** location within the **Core**.

---

### **Core Concepts**

#### **1. Global Infrastructure Logic**
In a clean **Enterprise Architecture**, interceptors are considered **headless logic** because they do not have a template and are registered globally in the **`provideCore()`** function. This allows them to monitor every outgoing and incoming request across all **features** and **patterns** without duplicating code.

#### **2. Resilience with Retries**
Network requests can fail for many reasons, such as temporary instability. By using **RxJS operators** like `retry()` within an interceptor, you can automatically attempt a failed request again before notifying the UI, significantly improving the **User Experience** in unstable environments.

#### **3. Performance with Caching**
Interceptors can be used to implement **Caching** by intercepting a request and returning a stored value instead of hitting the backend. This reduces the number of network calls and speeds up data delivery for frequently accessed, static information.

---

### **Hands-on Implementation**

#### **Building a Logging and Retry Interceptor**
This interceptor logs every request for debugging and automatically retries failed requests twice before giving up.

```typescript
// core/infrastructure/http.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';
import { retry, tap } from 'rxjs/operators';

export const loggingInterceptor: HttpInterceptorFn = (req, next) => {
  console.log(`🚀 Requesting: ${req.url}`);

  return next(req).pipe(
    // Automatically retry failed requests up to 2 times
    retry(2), 
    tap({
      next: (event) => console.log(`✅ Completed: ${req.url}`),
      error: (error) => console.error(`❌ Failed: ${req.url}`, error)
    })
  );
};
```

Register this in your **`core.ts`** using **`provideHttpClient(withInterceptors([...]))`**.

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Simple Logger**: Implement a functional interceptor that logs the **HTTP Method** (GET, POST, etc.) for every outgoing request.
2.  **Registration**: Register your new logging interceptor in the **`provideCore()`** function and verify its output in the browser console.

#### **Medium Challenges**
1.  **The Resilience Pattern**: Use the **RxJS `retry` operator** to create an interceptor that specifically retries failed GET requests but ignores failures for POST requests (to prevent duplicate submissions).
2.  **Infrastructure Extraction**: Move your interceptors into a dedicated **`core/infrastructure/`** folder to follow domain-based grouping.

#### **Hard Challenges**
1.  **The Signal Cache**: Build a **Caching Interceptor** that stores GET request results in a **Signal-based service** in the **Core**. If a request for the same URL is made again, return the cached signal value instead of performing a new HTTP call.
2.  **Timing Audit**: Create an interceptor that records the time when a request starts and ends, then logs the total **duration** in milliseconds to help identify slow API endpoints.
3.  **Architectural Proof**: Write a summary in your `readme.md` explaining why interceptors must never depend on a **Feature** or **Pattern**, maintaining the **One-Way Dependency Graph**.

