# Day 24: Internationalisation (i18n) & Localisation

### **Welcome Back!**
Congratulations on reaching **Day 24**! You are rapidly approaching the halfway mark of the **50 Days of Modern Angular** challenge. Having mastered fine-grained DOM behaviour yesterday, today we focus on making your application truly global. We are diving into **Internationalisation (i18n)** and **Localisation**, the processes that ensure your app can speak the language of any user, anywhere in the world.

---

### **Core Concepts**

#### **1. i18n vs. Localisation**
While often used interchangeably, these terms represent different stages of global readiness:
*   **Internationalisation (i18n):** The process of designing and developing your application so that it **can** be adapted to various languages and regions without engineering changes.
*   **Localisation:** The actual process of adapting your internationalised app for a specific region or language by adding **locale-specific components** and translating text.

#### **2. Build-Time vs. Runtime i18n**
In the Angular ecosystem, you generally have two paths for handling translations:
*   **Build-Time (`$localize`):** The official Angular way. You tag text in your templates, and the Angular compiler generates a separate version of your application for every supported language. This is highly performant as there is no translation overhead at runtime.
*   **Runtime (3rd Party Libraries):** Using libraries like `ngx-translate` or `transloco`. These load translation files dynamically as the user uses the app. While more flexible (no need for multiple builds), they add a small amount of runtime overhead.

#### **3. Architectural Integration**
In a clean **Enterprise Architecture**, infrastructure-specific providers—including those for **translations and logging**—should be registered globally within the **`provideCore()`** function in your `core.ts` or `app.config.ts`. This ensures that translation services are available eagerly from application startup.

#### **4. Right-to-Left (RTL) Support**
Modern styling tools like **Tailwind CSS** make supporting different text directions (LTR/RTL) much easier by using **logical properties**. Instead of hardcoding `margin-left`, you use utilities that adapt based on the document direction, ensuring your UI looks perfect for languages like Arabic or Hebrew.

---

### **Hands-on Implementation**

#### **Marking Text for Translation**
In your standalone component templates, use the `i18n` attribute to mark static text for the Angular extraction tool.

```html
<!-- feature/home/home.component.html -->
<section>
  <h1 i18n="@@homeGreeting">Welcome to our Global App!</h1>
  <p i18n="@@homeSubtext">Managing your data in every language.</p>
</section>
```

#### **Registering i18n in Core**
If you are using a 3rd party library for runtime translations, include its provider in your **`provideCore()`** setup.

```typescript
// core/core.ts
export function provideCore({ routes }: CoreOptions) {
  return [
    provideRouter(routes),
    // Example of registering a 3rd party translation provider
    importProvidersFrom(TranslateModule.forRoot()), 
    // ... other global providers
  ];
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The i18n Tag**: Go through your `MainLayout` component and add **`i18n` attributes** to at least three static text elements (e.g., footer text or menu items).
2.  **Meaning and Description**: Enhance your tags by adding a description and a meaning to help translators (e.g., `i18n="site header|The main title of the application@@homeHeader"`).

#### **Medium Challenges**
1.  **Code-Based Translation**: Research the **`$localize`** tag. Implement a signal-based greeting in a component's TypeScript logic that uses `$localize` to prepare a string for translation.
2.  **Locale Provider**: Research **`LOCALE_ID`**. Manually provide a specific locale (like `'fr'` for French) in your `app.config.ts` and verify that built-in Angular pipes (like `DatePipe` or `CurrencyPipe`) update their formatting automatically.

#### **Hard Challenges**
1.  **The RTL Switcher**: Use **Tailwind CSS logical properties** (like `ms-4` instead of `ml-4`) in a component. Create a button that toggles the `dir` attribute of the `body` tag between `ltr` and `rtl` to demonstrate how your layout adapts.
2.  **Extraction Workflow**: Research and run the **`ng extract-i18n`** command in your terminal. Examine the generated `.xlf` file and explain in your `readme.md` how this file would be used by a professional translation team.

