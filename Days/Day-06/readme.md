Day 6: Mastering Computed Signals and Effects in Modern Angular

1. Welcome Back & Progress Milestone

Congratulations on successfully completing Day 5 and gaining proficiency with writable signals [503, 507]. You are now ready to advance to "Level 2" of reactivity by mastering Computed Signals and Effects [503, 507]. These specific tools are essential for transforming raw data into a dynamic, responsive application that reacts to state changes automatically [503, 507].

2. Computed Signals: The Art of Derivation

Computed Signals are defined as read-only signals that derive their specific value from other signals through the use of the computed() function [494, 520]. These signals offer high technical efficiency because they are automatically recalculated only when their underlying dependencies undergo a change [494, 518]. By utilizing this derivation pattern, you ensure that your application logic remains declarative and highly performant [494, 518].

Computed Signals are strictly read-only, which means their values cannot be modified directly using methods like .set() or .update() [494, 520].

3. Effects: Handling Side Effects Gracefully

Effects are reactive functions designed to execute automatically whenever the signals they "read" are updated [509, 522]. They allow developers to perform operations that reach outside the Angular reactive system to synchronize with external state [509, 522].

Common use cases for using effect() include: [509, 522]

* Logging data for debugging or analytics purposes [509, 522].
* Synchronising application state with external APIs [509, 522].
* Updating and persisting data within localStorage [509, 522].

To function correctly, an effect must be created within a component's injection context, such as a constructor or as a field initializer [520, 521].

4. Hands-on Implementation: The Total Price Calculator

Follow these steps to implement a reactive price calculation logic within your component [521, 524].

Step 1: Create two writable signals to represent the quantity and price of an item [521, 524]. Step 2: Construct a computed signal named totalPrice that derives its value by multiplying the two writable signals [521, 524]. Step 3: Implement an effect() inside the constructor to log the totalPrice to the console whenever it recalculates [522].

import { Component, signal, computed, effect } from '@angular/core';

@Component({
  selector: 'app-price-calculator',
  template: `<div>Total: {{ totalPrice() }}</div>`
})
export class PriceCalculatorComponent {
  // Step 1: Define writable signals to manage internal state [521, 524].
  quantity = signal(1);
  price = signal(100);

  // Step 2: Define a computed signal to derive the total price [521, 524].
  totalPrice = computed(() => this.quantity() * this.price());

  constructor() {
    // Step 3: Add an effect to handle the console logging side effect [522].
    effect(() => {
      console.log(`Current Total Price: ${this.totalPrice()}`);
    });
  }
}


5. Day 6 Tiered Challenges

Easy

Construct a fullName computed signal that concatenates a firstName signal and a lastName signal with a space between them [494].

Medium

Develop a filteredItems computed signal that filters a list of items based on the current value of a searchTerm signal [494].

Hard

Implement a signal-based Logger that uses an effect() to automatically save a viewState signal to localStorage every time it is modified [522]. Ensure you use JSON.stringify() to properly serialize the state object before persistence to reinforce your understanding of side-effect handling [522].

6. Summary & Next Steps

Utilising Computed Signals and Effects ensures a predictable "one-way-ness" in the application's data flow [SOURCE_IMAGE_8]. This structural approach directly mirrors the principles of a clean dependency graph by separating raw state from derived values and side effects [122, 124]. By avoiding chaotic dependencies, you are building an architecture that scales effectively alongside your business requirements [122, 124, SOURCE_IMAGE_8]. You have made excellent progress today and are well-prepared to move forward to Day 7 [503].
