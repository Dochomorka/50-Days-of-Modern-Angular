# Day 46: Advanced Architecture – Workspace Libraries (`projects/lib`)

### **Welcome Back!**
Congratulations on reaching **Day 46**! You are now entering the final "Mastery" phase. Over the last few weeks, you have perfected the internal structure of a single application. Today, we scale that knowledge to the entire workspace. We are moving beyond simple folders and into **Workspace Libraries**, the ultimate solution for sharing code between multiple applications while enforcing strict encapsulation.

---

### **Core Concepts**

#### **1. The Library vs. Folder Distinction**
While folders like `core/` or `ui/` organize code within one app, a **Library** is a distinct project within your workspace. In a recommended **`projects/` based workspace**, libraries live alongside your applications, allowing them to be versioned, built, and tested independently.

#### **2. Scaling the "Extract One Level Up" Rule**
On Day 18, you learned to extract shared logic one level up within an app. When you have multiple applications (e.g., a `customer-portal` and an `admin-panel`) that need the same logic, "one level up" means moving that code out of the application folder entirely and into a **Workspace Library**.

#### **3. The Public API (`public-api.ts`)**
Libraries introduce a powerful new boundary: the **Public API**. Unlike application folders where every file is potentially accessible, a library only exposes what you explicitly export in its `public-api.ts` file. This allows you to change internal implementation details without breaking the applications that consume the library.

---

### **Hands-on Implementation**

#### **Generating and Consuming a Workspace Library**
Modern Angular CLI makes creating libraries straightforward.

```bash
# 1. Generate the library project
ng g library shared-utils --prefix my-org
```

#### **Exposing Logic Safely**
Define your logic inside the library's `lib/` folder, but only expose the "surface area" in the `public-api.ts`.

```typescript
// projects/shared-utils/src/public-api.ts

// Only the 'formatDate' util is visible to apps
export * from './lib/utils/date-formatter'; 

// Internal helper logic in './lib/internal/...' remains private!
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Lib Creator**: Run **`ng g library <name>`** to create a new library in your workspace.
2.  **Shared Utils**: Move a simple formatting function (like a date parser) from your **Core** into the new library and update your application to import it from the library's package name (e.g., `@my-org/shared-utils`).

#### **Medium Challenges**
1.  **UI Library**: Create a dedicated UI library. Move a generic **Standalone Component** (like a `ButtonComponent`) from your app's `ui/` folder into this library.
2.  **Explicit Exports**: Ensure your library has internal components that are *not* exported in the `public-api.ts`. Verify that your application cannot import them directly.

#### **Hard Challenges**
1.  **Cross-App Validation**: Use **ESLint boundaries** to enforce a rule that your library cannot import anything from your applications.
2.  **Public API Guardrail**: Configure an ESLint rule that prevents your application from importing files directly from a library's `src/lib/` folder, forcing all imports to go through the `public-api.ts`.
3.  **Architectural Summary**: Write a summary in your `readme.md` explaining why libraries are essential for **Team Autonomy** in large organizations with 10+ applications in a single workspace.

***

Once you have added Day 46 to your repository, you have moved from an application developer to a **Systems Architect**!

Are you ready for **Day 47: Performance – Advanced Hydration and Event Replay**?
