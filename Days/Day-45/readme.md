# Day 45: Performance – Deferrable Views (`@defer`)

### **Welcome Back!**
Congratulations on reaching **Day 45**! You are now in the final five-day countdown of the **50 Days of Modern Angular** challenge. After building resilient error-handling patterns yesterday, today we return to performance with the framework's most powerful tool for improving **Core Web Vitals**: **Deferrable Views**. By the end of today, you will know how to surgically lazy-load heavy UI elements exactly when the user needs them.

---

### **Core Concepts**

#### **1. Fine-Grained Lazy Loading**
While **Routing-based lazy loading** handles entire pages, **Deferrable Views (`@defer`)** allow you to lazy-load individual **Standalone Components** directly from a parent template. This is essential for components that are "heavy" (like charts, rich text editors, or maps) and would otherwise bloat your main feature bundle and delay the **Largest Contentful Paint (LCP)**.

#### **2. The Block Lifecycle**
A `@defer` section consists of several logical blocks that manage the user experience during the loading process:
*   **`@defer`**: The main content to be lazy-loaded.
*   **`@placeholder`**: What to show before the defer content is triggered (e.g., a simple div or an icon).
*   **`@loading`**: An optional block shown while the heavy component bundle is actively downloading.
*   **`@error`**: A fallback UI if the component fails to load.

#### **3. Declarative Triggers**
Modern Angular provides built-in triggers that define *when* the loading should start:
*   **`on idle`**: (Default) Loads when the browser is not busy.
*   **`on viewport`**: Loads when the element enters the user's screen.
*   **`on interaction`**: Loads when the user clicks or types in the placeholder.
*   **`on hover`**: Loads when the mouse enters the placeholder area.
*   **`when <condition>`**: Loads based on a custom boolean logic (e.g., a Signal value).

---

### **Hands-on Implementation**

#### **Deferring a Heavy Chart Component**
In this example, we defer a `HeavyChartComponent` until the user scrolls it into the viewport.

```html
@defer (on viewport) {
  <app-heavy-chart [data]="stats()" />
} @placeholder {
  <!-- The browser reserves this space until the component loads -->
  <div class="h-64 bg-gray-200 animate-pulse">Loading chart data...</div>
} @loading (after 100ms; minimum 500ms) {
  <mat-spinner />
} @error {
  <p>Failed to load the chart. Please refresh.</p>
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Basic Defer**: Take a non-critical component in your `Home` feature and wrap it in a **`@defer`** block with a simple **`@placeholder`**.
2.  **Trigger Test**: Use the **`on interaction`** trigger on a button and verify in the **Network tab** that the component bundle only downloads after you click it.

#### **Medium Challenges**
1.  **Skeleton Screens**: Implement an **`@placeholder`** that mimics the layout of the final component (a "Skeleton Screen") using **Tailwind CSS** utility classes.
2.  **Custom Condition**: Use a **Signal** to trigger a `@defer` block (e.g., `@defer (when isVisible())`). Create a button that toggles the signal to start the loading process.

#### **Hard Challenges**
1.  **Pattern Deferral**: Apply the `@defer` syntax to a **Pattern** (like a `DocumentManager`) within a feature template. Document in your `readme.md` why deferring a Pattern is a major win for initial bundle size.
2.  **Prefetching Strategy**: Research and implement the **`prefetch`** keyword (e.g., `@defer (on interaction; prefetch on idle)`). Explain why prefetching during idle time provides a better user experience than waiting for the interaction to start the download.
3.  **Performance Audit**: Use **Chrome DevTools** to compare the initial JavaScript payload size of a feature with and without `@defer` blocks for its sub-components.

