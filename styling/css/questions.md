### 1. The Box Model: What is the difference between `content-box` and `border-box`?
By default, browsers use `content-box`, where width and height only apply to the content. If you add padding or borders, the element grows larger than your set width. In `border-box`, the width includes content, padding, and borders.
* **Formula**: 
    $$Total\ Width = \text{set width (including padding/border)}$$
Most modern developers set `box-sizing: border-box` globally to make layouts more predictable.

---

### 2. Specificity: How does the browser decide which style wins?
CSS uses a weighting system. If multiple rules target the same element, the most specific one wins:
1.  **Inline styles**: (e.g., `style="..."`) - Highest weight.
2.  **IDs**: (e.g., `#header`)
3.  **Classes, attributes, and pseudo-classes**: (e.g., `.btn`, `[type="text"]`, `:hover`)
4.  **Elements and pseudo-elements**: (e.g., `h1`, `::before`) - Lowest weight.
`!important` overrides everything but is generally considered a "code smell" if overused.

---

### 3. Flexbox vs. Grid: When should you use one over the other?
* **Flexbox (1D)**: Best for a single row or column. It’s great for aligning items (like a navbar) or distributing space between elements.
* **Grid (2D)**: Best for complex layouts with both rows and columns. It allows you to align items to a precise coordinate system.
> **Rule of thumb**: Use Grid for the main page layout and Flexbox for the components inside it.



---

### 4. Stacking Contexts: Why does `z-index: 999` sometimes fail?
A `z-index` only works within its **Stacking Context**. If a parent element has a `z-index` or certain properties (like `opacity < 1`, `transform`, or `position: relative`), it creates a new layer. An element inside that layer can never move "behind" or "in front of" elements in a different stacking context, no matter how high its `z-index` is.

---

### 5. Custom Properties (CSS Variables): Why use them over Sass variables?
Unlike Sass variables (which are compiled away into static values), CSS variables (`--my-color`) are **dynamic** and **live** in the DOM. 
* They can be changed via JavaScript.
* They respect the cascade (you can redefine a variable inside a specific class).
* They are perfect for implementing "Dark Mode" by simply swapping variable values on the `:root`.

---

### 6. The `display: none` vs. `visibility: hidden` vs. `opacity: 0`
* **`display: none`**: The element is removed from the document flow; it takes up no space.
* **`visibility: hidden`**: The element is invisible, but it still takes up space in the layout.
* **`opacity: 0`**: The element is invisible and takes up space, but it is still **interactive** (you can still click a button that has 0 opacity).

---

### 7. CSS Architecture: What is BEM and why is it used?
**BEM** (Block, Element, Modifier) is a naming convention (e.g., `.card__button--large`). 
* **Benefit**: It prevents "selector leakage" where styles unintentionally affect other parts of the site. It makes CSS modular and easier to read for teams.

---

### 8. Responsive Design: `em` vs. `rem` units
* **`rem` (Root EM)**: Relative to the font size of the `<html>` element. It’s consistent throughout the app.
* **`em`**: Relative to the font size of its **parent**. It is useful for components that should scale proportionally (like a button’s padding scaling with its text size).

---

### 9. Aspect Ratio: How do you maintain a 16:9 container?
Before the modern `aspect-ratio` property, we used the "Padding Hack" (setting `padding-top: 56.25%`). Now, we can simply use:
```css
.video-container {
  aspect-ratio: 16 / 9;
  width: 100%;
}
```
This ensures the element maintains its shape regardless of the width.

---

### 10. The `:has()` Pseudo-class: Why is it called the "Parent Selector"?
For years, CSS couldn't "style a parent based on its children." The new `:has()` selector changes this. 
* **Example**: `card:has(img)` will only apply styles to a card if it contains an image. It effectively allows us to logic-check the DOM structure using only CSS.