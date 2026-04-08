### 1. Nesting: How does it improve readability and what is the "Rule of Three"?
Nesting allows you to mirror your HTML structure within your style sheet.
* **The Benefit**: It keeps related styles grouped together.
* **The Rule of Three**: It is generally advised not to nest more than **three levels deep**. Excessive nesting creates overly specific CSS selectors that are hard to override and bloating to the final CSS file.

---

### 2. The Ampersand (`&`): How is the Parent Selector used?
The `&` operator references the outer selector. It is commonly used for:
* **Pseudo-states**: `&:hover` or `&:focus`.
* **BEM Naming**: If the parent is `.block`, `&__element` compiles to `.block__element`.
* **Qualifying Selectors**: `.dark-theme &` allows you to change a component's style when it's inside a specific theme class.

---

### 3. Mixins vs. Placeholders: When should you use `@mixin` vs `%placeholder`?
* **`@mixin`**: Used for reusable blocks of code that require **arguments** (e.g., a flexbox center tool where you can toggle orientation).
* **`%placeholder` (with `@extend`)**: Used when you want multiple classes to **share the exact same styles**. SCSS will group the selectors together in the compiled CSS (e.g., `.btn, .link, .nav { ... }`), which reduces code duplication.

---

### 4. Partials and `@use`: How do you organize a large SCSS project?
You use **Partials**—files starting with an underscore (e.g., `_variables.scss`). These are not compiled into their own CSS files.
* **`@use`**: The modern way to load partials. It keeps variables and mixins **namespaced**. Instead of just `$primary-color`, you would use `variables.$primary-color`, preventing naming conflicts across large teams.

---

### 5. Control Flow: How do `@if`, `@each`, and `@for` work?
SCSS allows for basic logic:
* **`@if`**: Apply styles based on conditions (e.g., light vs. dark mode).
* **`@each`**: Iterate through a list or map. Great for generating icon classes or social media brand colors.
* **`@for`**: Useful for generating nth-child animations or a grid system where you need incrementing numbers.

---

### 6. Functions: How do they differ from Mixins?
* **Mixins** return **styles** (properties and values).
* **Functions** return a **single value**. 
For example, you might write a function to calculate the $z-index$ from a map or a function that converts pixels to `rem`.

---

### 7. Maps and Lists: How do you handle complex data?
SCSS provides data structures:
* **Lists**: Simple sequences like `10px 20px 10px 5px`.
* **Maps**: Key-value pairs like `( "primary": #333, "secondary": #999 )`.
Maps are incredibly powerful for creating theme systems where you can look up values dynamically using `map-get()`.

---

### 8. Inheritance: The dangers of `@extend`
While `@extend` is great for keeping CSS "DRY" (Don't Repeat Yourself), it can be dangerous. If you extend a selector that is nested elsewhere, it can create a "CSS explosion"—a massive chain of selectors that makes your stylesheet difficult to debug and increases file size unexpectedly.

---

### 9. Operations and Math: What can SCSS calculate?
SCSS can perform math on numbers with compatible units:
* **Examples**: `width: 100% / 3;` or `margin-top: $variable + 10px;`.
* **Caution**: While SCSS does math at **compile-time**, CSS `calc()` does math at **run-time**. If you need to mix units (like `100% - 20px`), you must use the native CSS `calc()`.

---

### 10. The `@forward` Rule: Why is it used in library development?
When building a design system, `@forward` acts like a relay. It allows you to collect multiple partials into a single "entry point" file without adding extra layers of namespacing. This makes it easier for consumers of your library to access all your mixins and variables with a single `@use` statement.
