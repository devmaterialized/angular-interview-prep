# Angular Interview Questions & Answers for Senior Developers

## 1. Change Detection: Default vs. OnPush vs. Signals

**Direct Answer:** Traditionally, Angular uses Zone.js to trigger change detection globally. **Default** strategy checks the entire component tree on any event, while **OnPush** limits checks to input changes or manual triggers. **Signals** represent the future of Angular, enabling fine-grained, zone-less reactivity where only the specific DOM nodes bound to a signal are updated.

**Deeper Dive:** For a senior, the discussion should center on **Zone.js overhead**. `OnPush` reduces the number of components checked but still relies on a top-down dirty-checking sweep. Signals bypass this by creating a dependency graph. This allows Angular to eventually remove Zone.js entirely (Zoneless mode), leading to faster initial loads and better runtime performance.

**Analogy:**
* **Default:** A **Security Guard** who walks through every single room in the building every time a door opens anywhere.
* **OnPush:** A **Security Guard** who only checks a room if the occupant calls them or if a new piece of furniture is delivered to that specific room.
* **Signals:** A **Smart Home Sensor**. When a specific light bulb burns out, only that bulb sends a notification to the repairman. No guard needed.

**Code Example:**
```typescript
// Modern Signal-based Component
@Component({
  template: `<h1>{{ title() }}</h1>`,
  standalone: true
})
export class App {
  title = signal('Fintech Dashboard');

  update() {
    this.title.set('New Reports'); // Fine-grained update
  }
}
```

---

## 2. Standalone Components: Why the shift away from NgModules?

**Direct Answer:** Standalone components eliminate the need for `NgModules`, making components the primary unit of reuse and organization. This simplifies the mental model, reduces "boilerplate" code, and enables better tree-shaking and lazy loading.

**Deeper Dive:** NgModules often led to "Shared Module" bloat, where a component might import a massive module just for a single pipe. Standalone components allow for "Component-First" architecture. From a senior perspective, this facilitates micro-frontend architectures and makes testing easier, as dependencies are explicitly listed at the component level.

**Analogy:** **NgModules** are like **Department-wide Supply Closets**—everyone in the department has access to everything, even if they don't need it. **Standalone Components** are like **Personalized Toolkits**—each worker carries exactly what they need for their specific job, making them more mobile and efficient.

**Code Example:**
```typescript
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CommonModule, RouterOutlet, UserListComponent],
  template: `<app-user-list />`
})
export class AppComponent {}
```

---

## 3. Dependency Injection: How does the Hierarchical Injector work?

**Direct Answer:** Angular's DI system is hierarchical. If a dependency isn't found in the current component's injector, Angular searches up through the parent components' injectors until it reaches the Root or Platform injector.

**Deeper Dive:** Seniors should leverage this for **Service Isolation**. For instance, providing a service at the component level creates a fresh instance for that component and its children, which is perfect for complex forms or stateful sub-trees. Using `providedIn: 'root'` is standard for singletons, but understanding `providedIn: 'platform'` (shared across multiple apps in a workspace) or `providedIn: SomeModule` is a sign of deep expertise.

**Analogy:** It’s like **Corporate Budgeting**. A department can have its own "Special Project Budget" (Component Provider). If they need something that isn't in their specific budget, they look to the "Regional Budget" (Parent Provider), and finally to the "Global Headquarters" (Root Provider).

**Code Example:**
```typescript
@Component({
  providers: [LocalStateService] // Instance unique to this component
})
export class AccountDetailsComponent {}
```

---

## 4. Performance Optimization: Beyond Change Detection

**Direct Answer:** Beyond choosing the right CD strategy, optimization involves efficient DOM management (`trackBy`), lazy loading, image optimization (NgOptimizedImage), and minimizing the bundle size through tree-shaking.

**Deeper Dive:** A senior should mention **Deferred Loading** (`@defer`). This new control flow block allows developers to declaratively delay the loading of heavy components until certain conditions are met (e.g., when they enter the viewport or during browser idle time). This is a game-changer for Core Web Vitals (LCP and FID).

**Analogy:** It's like **Just-In-Time (JIT) Inventory Management** in a factory. You don't store every single part for every product on the assembly line at once (loading everything). You only bring the heavy parts (heavy components) to the floor exactly when the machine is ready to install them (@defer).

**Code Example:**
```html
@defer (on viewport) {
  <app-heavy-chart [data]="stats" />
} @placeholder {
  <div>Loading chart...</div>
}
```

---

## 5. Security: How does Angular protect against XSS?

**Direct Answer:** Angular treats all values as untrusted by default. It automatically sanitizes data bound to the DOM (via properties like `[innerHTML]`) to prevent Cross-Site Scripting (XSS) attacks.

**Deeper Dive:** Angular uses a `DomSanitizer` to categorize values into `Html`, `Style`, `Script`, `Url`, and `ResourceUrl`. If you *must* bypass security (e.g., for a trusted third-party iframe), you must explicitly use methods like `bypassSecurityTrustHtml`. A senior knows that this should be used sparingly and that Content Security Policy (CSP) headers are a crucial second line of defense.

**Analogy:** Angular acts like a **Compliance Officer** at a bank. Every document (data) that comes from the outside is scrutinized and "redacted" (sanitized) to remove any dangerous instructions before it's allowed to be filed in the official records (the DOM).

**Code Example:**
```typescript
constructor(private sanitizer: DomSanitizer) {
  // Dangerous: Only do this for trusted content!
  this.trustedUrl = this.sanitizer.bypassSecurityTrustUrl(userSuppliedUrl);
}
```
