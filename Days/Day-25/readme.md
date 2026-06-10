# Day 25: Performance – Image Optimisation (`NgOptimizedImage`)

### **Welcome Back!**
Congratulations on reaching **Day 25**! You have officially hit the **halfway point** of the **50 Days of Modern Angular** challenge. You have mastered the architecture, the state management, and the global readiness of your application; today, we focus on making it feel instant by optimising how it handles its heaviest assets: **Images**.

---

### **Core Concepts**

#### **1. The Weight of Images**
Images are often the largest resources a browser has to download. If not managed correctly, they can lead to poor **Core Web Vitals**, specifically the **Largest Contentful Paint (LCP)**, which measures when the largest visual element becomes visible to the user. Unoptimised images can cause significant bottlenecks in application startup times.

#### **2. What is `NgOptimizedImage`?**
Modern Angular introduced the **`NgOptimizedImage`** directive (using the `ngSrc` attribute) to enforce image loading best practices automatically. It is not just a replacement for the standard `<img>` tag; it is a performance engine that protects your application from common pitfalls like layout shifts and unoptimised delivery.

#### **3. Key Performance Protections**
*   **Enforced Dimensions:** Requires `width` and `height` (or a `fill` mode) to prevent **Cumulative Layout Shift (CLS)**, ensuring the browser reserves the correct space before the image loads.
*   **Intelligent Lazy Loading:** Images are lazy-loaded by default, meaning they only download when they are close to the user's viewport.
*   **Priority Marking:** You can mark "above-the-fold" images with the **`priority`** attribute. This tells Angular to preload the image and ensures it is not lazy-loaded, directly improving the **LCP** score.
*   **Preconnect Hints:** The directive can automatically generate preconnect links for your image domains, speeding up the connection setup for external image hosts.

---

### **Hands-on Implementation**

#### **Using `ngSrc` in a Standalone Component**
To use this feature, you must import the directive into your component's **template context**.

```typescript
import { Component } from '@angular/core';
import { NgOptimizedImage } from '@angular/common';

@Component({
  standalone: true,
  selector: 'app-hero-header',
  imports: [NgOptimizedImage], // Import standard directive
  template: `
    <header class="relative h-64">
      <!-- High priority image for LCP -->
      <img ngSrc="assets/brand-hero.webp" 
           width="1200" height="600" 
           priority 
           class="object-cover">
    </header>
    
    <section>
      <!-- Standard optimized image (lazy-loaded by default) -->
      <img ngSrc="assets/feature-icon.png" width="100" height="100">
    </section>
  `
})
export class HeroHeaderComponent {}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Simple Upgrade**: Identify an `<img>` tag in your project and refactor it to use **`ngSrc`**. Ensure you provide the mandatory `width` and `height` attributes to prevent layout shifts.
2.  **Import Check**: Verify that your standalone component correctly imports **`NgOptimizedImage`** from `@angular/common` and that your local assets are being served correctly.

#### **Medium Challenges**
1.  **LCP Optimisation**: Identify the most prominent image on your landing page. Apply the **`priority`** attribute and verify in the Chrome DevTools **Network tab** that the browser initiates the download earlier than other images.
2.  **Aspect Ratio Safety**: Try to use `ngSrc` without providing `width` or `height`. Observe the **console error** Angular provides and explain in your `readme.md` why this strictness is beneficial for maintaining a stable UI.

#### **Hard Challenges**
1.  **Image Loader Configuration**: Research **"Image Loaders"** in the Angular documentation. Configure a loader in your `app.config.ts` for a service like Cloudinary or Imgix to see how Angular automatically generates a **`srcset`** for different screen sizes.
2.  **Performance Profiling**: Use the **Chrome DevTools Performance Tab** to compare the "Evaluate Script" time and LCP score of a page using standard `src` versus one using `ngSrc`.
3.  **Architectural Placement**: Write a summary explaining why generic UI components (like a `UserAvatar`) should always use **`ngSrc`** to ensure that every feature consuming that UI component benefits from optimised loading automatically.

