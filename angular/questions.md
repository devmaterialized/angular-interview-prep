## 1. List change detection strategies and when to use each one.

**Direct Answer:** Angular traditionally offers two strategies: **Default** (CheckAlways) and **OnPush**. **Default** checks the entire tree on every event, while **OnPush** only checks if inputs change, an event originates in the component, or an observable (with async pipe) emits.

**Deeper Dive:** For seniors, the discussion extends to **Signals** and **Zoneless** mode. `OnPush` reduces the check surface but still relies on Zone.js. Signals enable fine-grained reactivity, allowing Angular to update only specific DOM nodes without a full top-down tree sweep. In the future, this allows for the removal of Zone.js entirely.

**Analogy:**
- **Default:** A **Security Guard** who walks through every single room in the building every time a door opens anywhere.
- **OnPush:** A **Security Guard** who only checks a room if the occupant calls them or if a new piece of furniture is delivered.
- **Signals:** A **Smart Home Sensor**. When a specific bulb burns out, only that bulb sends a notification. No guard needed.

**Code Example:**
```typescript
@Component({
  selector: 'app-user',
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `{{ user().name }}`
})
export class UserComponent {
  user = input.required<User>(); // Signal-based input
}
```

---

## 2. Standalone Components: Why the shift away from NgModules?

**Direct Answer:** Standalone components eliminate the need for `NgModules`, making components the primary unit of reuse. This simplifies the mental model and enables better tree-shaking and lazy loading.

**Deeper Dive:** NgModules often led to "Shared Module" bloat. Standalone components allow for "Component-First" architecture, where dependencies are explicitly listed at the component level. This makes testing easier and facilitates micro-frontend architectures.

**Analogy:** **NgModules** are like **Department-wide Supply Closets**—everyone has access to everything. **Standalone Components** are like **Personalized Toolkits**—each worker carries exactly what they need, making them more mobile and efficient.

**Code Example:**
```typescript
@Component({
  standalone: true,
  imports: [CommonModule, UserListComponent],
  template: `<app-user-list />`
})
export class AppComponent {}
```

---

## 3. Dependency Injection: How does the Hierarchical Injector work?

**Direct Answer:** Angular's DI system is hierarchical. If a dependency isn't found in the current component's injector, Angular searches up through parent injectors until it reaches the Root or Platform level.

**Deeper Dive:** Seniors leverage this for **Service Isolation**. Providing a service at the component level creates a fresh instance for that component and its children, which is perfect for complex forms or stateful sub-trees.

**Analogy:** It’s like **Corporate Budgeting**. A department can have its own "Special Project Budget" (Component Provider). If they need more, they look to the "Regional Budget" (Parent), and finally to "Global Headquarters" (Root).

**Code Example:**
```typescript
@Component({
  providers: [LocalStateService] // Unique instance for this subtree
})
export class AccountDetailsComponent {}
```
