# Day 30: End-to-End (E2E) Testing with Playwright

### **Welcome Back!**
**Congratulations** on reaching **Day 30**! You have officially completed 60% of the **50 Days of Modern Angular** challenge. Yesterday, we focused on verifying individual units of logic and signal-based state. Today, we step into the shoes of the user. We are mastering **End-to-End (E2E) Testing** with **Playwright**, the modern industry standard for ensuring that your **isolated features**, **core logic**, and **layouts** all work together perfectly in the browser.

---

### **Core Concepts**

#### **1. Testing the "Base Reality"**
Unit tests check the code, but E2E tests check the **"Base Reality"**—the browser. An E2E test automates a real browser to click buttons, fill forms, and navigate routes just like a human would. This is the only way to verify that your **eagerly loaded core** and **lazy-loaded features** are bootstrapping correctly and communicating without runtime errors like the `NullInjectorError`.

#### **2. The "Black Box" Advantage**
In our **Enterprise Architecture**, we treat every lazy feature as a **"black box"**. E2E testing is uniquely suited for this model because it doesn't care about the internal implementation or whether you use **Signals** or **RxJS**. It only cares that when a user interacts with the **Feature**, the expected result appears in the **UI**.

#### **3. Why Playwright?**
While older tools were slow and flaky, **Playwright** is built for the modern web. It supports:
*   **Auto-waiting**: It waits for elements to be actionable, reducing "flaky" tests.
*   **Trace Viewer**: You can record and "rewatch" a failed test execution to see exactly what happened in the DOM.
*   **Cross-Browser**: It tests your app in Chromium, Firefox, and WebKit (Safari) with a single command.

---

### **Hands-on Implementation**

#### **Writing Your First User Flow**
In a modern workspace, your E2E tests live in a dedicated folder (often `e2e/`). Let's verify that our **MainLayout** and **Home Feature** are working correctly.

```typescript
import { test, expect } from '@playwright/test';

test.describe('Initial Application Load', () => {
  test('should redirect to home and display the layout', async ({ page }) => {
    // 1. Visit the root URL
    await page.goto('/');

    // 2. Verify the redirect to /home
    await expect(page).toHaveURL(/.*home/);

    // 3. Check for our organization's logo in the layout
    const logo = page.locator('img[alt="Angular Experts Logo"]');
    await expect(logo).toBeVisible();

    // 4. Verify the lazy feature component rendered its "works" text
    await expect(page.getByText('home works!')).toBeVisible();
  });
});
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Title Assert**: Write a test that navigates to your app and verifies the document title matches what you set in your `app.component.ts`.
2.  **Navigation Check**: Use Playwright to click a navigation button in your **`mat-toolbar`** and verify the URL changes to the correct feature route.

#### **Medium Challenges**
1.  **Form Interaction**: Create a test for a `Login` feature. Fill in an email and password field, click "Submit", and verify that a "Welcome" message appears (simulating a successful state change).
2.  **Responsive Layout Test**: Use Playwright's `viewport` option to test your app on a mobile-sized screen. Verify that the "Desktop Logo" is hidden and the "Mobile Icon" is visible in your **MainLayout**.

#### **Hard Challenges**
1.  **The "Throw-Away" Validation**: Prove the **Isolation** of your architecture. Intentionally break the logic inside a **Lazy Feature** but keep its **Output** signatures the same. Show that your E2E test fails, but your **Layout** and **Core** unit tests still pass.
2.  **Fractal Flow Test**: Write a complex test that navigates through a three-level **Fractal Lazy Route** (e.g., `App` -> `Feature` -> `Sub-Feature`). Verify that the breadcrumbs in your UI update correctly at each step.
3.  **Network Mocking**: Research Playwright's `route.fulfill()` method. Build a test that mocks a failed 500 error from your API and verify that your **Global ErrorHandler** in the **Core** displays the correct error UI to the user.

