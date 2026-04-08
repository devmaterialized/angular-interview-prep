### 1. Utility-First: What is the core philosophy of Tailwind?
Unlike Bootstrap (which gives you pre-built components like `.btn`), Tailwind gives you **atomic utility classes** (like `flex`, `pt-4`, `text-center`). 
* **The Goal**: To build custom designs without ever leaving your HTML file and without writing a single line of custom CSS.

---

### 2. The JIT (Just-In-Time) Engine: How does it handle file size?
In older versions, Tailwind generated thousands of classes upfront, leading to massive CSS files. The **JIT engine** scans your project files (HTML, JS, TS) and generates the CSS **only for the classes you actually use**.
* **Result**: Your production CSS bundle is usually incredibly small (often under 10kb), regardless of how large your project grows.

---

### 3. Arbitrary Values: How do you use a specific value like `top: 13px`?
Tailwind provides a "design system" out of the box, but sometimes you need a one-off value. You use **square bracket notation**:
* **Example**: `top-[13px]` or `bg-[#1da1f2]`.
This allows you to stay within the utility-first workflow without jumping back to a CSS file for "magic numbers."

---

### 4. Responsiveness: How do mobile-first breakpoints work?
Tailwind uses a **mobile-first** approach. Unprefixed classes apply to all screen sizes, while prefixed classes (like `md:`, `lg:`) apply at that breakpoint and above.
* **Example**: `w-full md:w-1/2` means the element is full-width on mobile but takes up half the screen on tablets and desktops.

---

### 5. Hover, Focus, and Other States: How are they handled?
Instead of writing `:hover` selectors in CSS, you prefix the utility class with the state name.
* **Example**: `bg-blue-500 hover:bg-blue-700 focus:outline-none`.
You can even stack them, like `dark:hover:bg-gray-800`.

---

### 6. The `tailwind.config.js`: Why is it the "Heart" of the project?
This file is where you define your **Design System**. You can:
* Extend the default theme (adding custom colors or fonts).
* Override the default spacing scale.
* Register plugins (like `@tailwindcss/typography`).
It ensures that your entire team uses the same "source of truth" for design tokens.

---

### 7. Reusability: Should you use `@apply` for everything?
The `@apply` directive allows you to extract common utility patterns into a CSS class. 
* **The Trap**: Overusing `@apply` turns Tailwind back into traditional CSS, losing the benefits of the JIT engine and quick debugging. 
* **Better Way**: Use **Component Extraction** (creating a reusable React/Vue/Angular component) instead of CSS abstractions.

---

### 8. Group and Peer Selectors: How do you style a child based on a parent's state?
* **`group`**: Add `group` to a parent and `group-hover:text-white` to a child. When the parent is hovered, the child changes color.
* **`peer`**: Style an element based on the state of a **sibling**. For example, showing an error message only when an input is in an `invalid` state.

---

### 9. Dark Mode: How is it implemented?
Tailwind has built-in support for dark mode using the `dark:` variant.
* **Media Strategy**: Uses the user's OS preference (`prefers-color-scheme`).
* **Class Strategy**: Uses a `.dark` class added to the `<html>` or `<body>` tag, which is perfect for manual "Dark Mode" toggles in your UI.

---

### 10. Preflight: Why does my `<h1>` look like a normal `<p>`?
Tailwind includes **Preflight**, a set of base styles that "resets" the browser's default styling. It removes margins, font weights, and headings sizes.
* **Reason**: This provides a "blank slate" so your design looks identical across all browsers, forcing you to be intentional about your typography using classes like `text-2xl` and `font-bold`.


