# Day 49: Deployment – CI/CD, Budgets, and Production Hardening

### **Welcome Back!**
Congratulations on reaching **Day 49**! You are just one step away from completing the full **50 Days of Modern Angular** journey. After mastering the extreme scalability of Micro-Frontends yesterday, today we focus on the "last mile": getting your code into production safely and keeping it performant over time. We will focus on **CI/CD integration**, **Performance Budgets**, and **Production Hardening** to ensure your architectural integrity is never compromised.

---

### **Core Concepts**

#### **1. Automated Guardrails in CI/CD**
In our **Enterprise Architecture**, the most critical part of the CI/CD pipeline isn't just building the code—it's **Automated Validation**. By running `ng lint` in your pipeline, you ensure that every pull request adheres to your **One-Way Dependency Graph**. If a developer accidentally imports a **Feature** into the **Core**, the CI pipeline fails, acting as an automated "senior reviewer" that prevents architectural rot.

#### **2. Enforcing Performance with Budgets**
Enterprise applications often grow in size until they become slow. **Bundle Budgets** are a feature of the Angular CLI that allows you to set hard limits on your bundle sizes in `angular.json`. If an update causes a bundle to exceed its limit, the build will fail with an error, forcing developers to optimize or use **Deferrable Views** before the code can be deployed.

#### **3. Production Hardening and Environments**
"Hardening" an app means preparing it for the harsh reality of production. This includes using **Environment Files** (`environment.ts` vs `environment.development.ts`) to manage API keys and backend URLs securely. It also involves ensuring your application uses **OnPush Change Detection** and **Zoneless** configurations where possible to maximize runtime performance.

---

### **Hands-on Implementation**

#### **Configuring a "Maximum Error" Budget**
Modify your `angular.json` to ensure your initial eager bundle never exceeds a specific size, preventing "accidental" large imports of 3rd party libraries.

```json
// angular.json
"configurations": {
  "production": {
    "budgets": [
      {
        "type": "initial",
        "maximumWarning": "500kb",
        "maximumError": "1mb" 
      }
    ],
    // ...
  }
}
```

#### **CI/CD Validation Script**
Add a strict linting script to your `package.json` that you can run in GitHub Actions, GitLab CI, or Jenkins.

```json
// package.json
"scripts": {
  "ci:validate": "ng lint && npm run format:test && ng test --watch=false --browsers=ChromeHeadless"
}
```

---

### **Tiered Challenges**

#### **Easy Challenges**
1.  **The Budget Trigger**: Lower your `maximumError` budget in `angular.json` to a very small number (e.g., `100kb`) and run `ng build`. Verify that the build fails as expected.
2.  **Environment Setup**: Run **`ng g environments`** and set a `baseUrl` for your production and development environments.

#### **Medium Challenges**
1.  **Architectural CI Simulation**: Intentionally break an architecture rule (like importing a Feature into the Core). Run your **`ci:validate`** script and verify it catches the violation.
2.  **Production Mapping**: Update your `AuthInterceptor` from Day 42 to use the `environment.baseUrl` instead of a hardcoded string.

#### **Hard Challenges**
1.  **The "Budget vs. Defer" Analysis**: Identify a heavy 3rd party library (like a chart lib). Show how using **`@defer`** allows you to keep your **Initial Budget** low while still using the heavy library in a feature.
2.  **CI/CD Workflow Design**: Write a sample YAML configuration for a GitHub Action that installs dependencies, runs architectural validation, and only deploys to a "Staging" environment if all lint rules pass.
3.  **Final Hardening Summary**: Write a summary in your `readme.md` explaining why **Automated Budgets** and **Boundary Rules** are 5 times more effective at preserving long-term project health than manual code reviews alone.
