# Day 26: Performance – Advanced Bundle Analysis

### **Welcome Back!**
Congratulations on reaching **Day 26**! You have officially entered the **second half** of the **50 Days of Modern Angular** challenge. After mastering image optimisation yesterday, today we turn our focus to the "hidden" weight of your application: the **JavaScript bundles**. By the end of today, you will know how to surgically inspect your application's output, identify bloated dependencies, and set up automated "guardrails" to keep your app fast for the long term.

---

### **Core Concepts**

#### **1. The Cost of Parsing and Execution**
While network bandwidth is often high, the **parsing and execution** of JavaScript remains the primary bottleneck for startup performance, especially on mobile devices. Every kilobyte of code you ship must be evaluated by the browser before your app becomes interactive. Large **eager bundles** (code loaded at startup) have the most significant negative impact on **Core Web Vitals**.

#### **2. Bundle Visualisation Tools**
To fix bloat, you must first see it. We use two industry-standard tools for this:
*   **`esbuild-visualizer`**: Perfect for modern **esbuild**-based Angular projects, it creates an interactive **treemap** or **sunburst** chart showing which files and libraries take up the most space.
*   **`source-map-explorer` (SME)**: This tool provides a deeper dive by looking at the generated **source maps**, allowing you to see exactly how much of a library's code was actually included after **tree-shaking**.

#### **3. Budgets as Guardrails**
You shouldn't have to check your bundle size manually every day. Angular CLI **Budgets** allow you to define thresholds in your `angular.json` file. If a change (like importing a heavy 3rd-party library) pushes your bundle over a limit, the build will provide a **warning** or an **error**, acting as a unit test for your application's performance.

---

### **Hands-on Implementation**

#### **Step 1: Install the Analysis Suite**
You will need the visualisers and a small server to host the generated reports.

```bash
npm install -D esbuild-visualizer source-map-explorer http-server
```

#### **Step 2: Create the Analysis Script**
Add a script to your `package.json` that builds the app with statistics enabled and launches the visualiser.

```json
"scripts": {
  "analyze": "ng build --stats-json --output-hashing none --named-chunks && esbuild-visualizer --template treemap --metadata dist/example-app/stats.json --filename dist/example-app/analyze/index.html && http-server -o -c-1 ./dist/example-app/analyze/"
}
```

*Note: The `--named-chunks` flag makes the resulting report much easier to read by using human-readable filenames instead of hashes.*

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Initial Audit**: Run your new `npm run analyze` script and identify the single largest library in your **eager main bundle**.
2.  **Report Formats**: Research the `--template` flag for `esbuild-visualizer`; try generating a **sunburst** or **network** chart instead of a treemap.

#### **Medium Challenges**
1.  **Enforcing Budgets**: Open your `angular.json` and set a `maximumWarning` of **500kb** for the `initial` bundle in your production configuration.
2.  **Trigger the Guardrail**: Eagerly import a large library (like `moment` or a heavy part of `lodash`) into your `AppComponent` and verify that `ng build` now triggers a **bundle budget warning**.

#### **Hard Challenges**
1.  **Tree-Shaking Investigation**: Use `source-map-explorer` to inspect a 3rd-party UI library you are using. Identify if any "dead code" (parts of the library you aren't using) is still being bundled and explain in your `readme.md` why tree-shaking might have failed (e.g., side effects).
2.  **Virtual Pre-bundle Analysis**: Research how the bundler extracts shared logic into "virtual pre-bundles". Document why this is more efficient than including the shared logic in every single **lazy feature bundle** that consumes it.
3.  **Performance Profiling**: Use the **Chrome DevTools Performance Tab** to find the `Evaluate Script` block. Document the execution time and compare it to the total size of your eager bundle.

