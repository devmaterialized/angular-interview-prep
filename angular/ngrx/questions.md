### 1. The Redux Pattern: Data Flow
The data flow in NgRx is a **unidirectional loop**.
1.  **Action**: An event is dispatched (e.g., a button click).
2.  **Reducer**: The Store sees the action and calls the Reducer. The Reducer takes the *current state* and the *action*, then calculates a *new state*.
3.  **Store**: The Store updates its internal state and notifies all subscribers (Components/Selectors).

---

### 2. Immutability: Why not `state.user = newUser`?
Change detection in Angular (and Redux patterns) relies on **reference equality**. If you mutate an object property, the memory reference of the state remains the same. The Store won't "know" the state changed, so your UI won't update. By returning a *new* object, you trigger the "push" notification to the rest of the app.

---

### 3. Actions: Props and Descriptions
* **Props**: These are the metadata or payloads attached to an action (e.g., the ID of an item to delete).
* **Descriptions**: Action types (e.g., `[User Page] Load Users`) should be descriptive to make the **Redux DevTools** readable. It acts as a "journal" of everything that happened in the app.

---

### 4. Selectors: The Benefit
Selectors provide **encapsulation**. If you use `store.select('user')` everywhere and later change your state structure to `state.auth.user`, you have to refactor every component. With a selector, you change the logic in **one place**.

---

### 5. Store DevTools: Debugging
You use the **Redux DevTools** browser extension to:
* **Time Travel**: Jump back to a previous state to see exactly when a bug occurred.
* **Action Diffing**: See exactly which part of the state changed after a specific action.
* **State Export**: Download the state of a bugged session and re-import it to your local environment.

---

### 6. Effects vs. Reducers
* **Reducers**: For **pure, synchronous** state changes. No API calls, no side effects.
* **Effects**: For **asynchronous** tasks or side effects (API calls, logging, navigation, or accessing external browser APIs).

---

### 7. Memoization and Re-calculation
NgRx selectors are **memoized**, meaning they remember the last result.
* **Optimization**: If the input state hasn't changed, the selector returns the cached result without re-executing the logic.
* **Re-calculation**: A selector only re-calculates when one of its **input slices** of state changes.

---

### 8. `createEffect` with `{ dispatch: false }`
Use this for "Fire and Forget" side effects that don't need to update the store, such as:
* Logging to an external service.
* Navigating to a different route via the Router.
* Opening a snackbar/toast notification.

---

### 9. Feature Stores: Lazy Loading
To keep the initial bundle small, you use `StoreModule.forFeature('featureName', reducer)` and `EffectsModule.forFeature([Effects])` inside your **lazy-loaded Angular modules**. This injects the state slice into the global store only when that module is loaded.

---

### 10. @ngrx/entity
The Entity Adapter solves the "List vs. Dictionary" problem. 
* **Problem**: Finding an item in a large array ($O(n)$) is slow.
* **Solution**: It stores data as a collection of IDs and a dictionary of entities ($O(1)$ lookup). It provides built-in helpers like `addOne`, `updateOne`, and `removeMany` to simplify CRUD logic.

---

### 11. Component Store vs. Global Store
* **Global Store**: For state shared across the *entire* app (User profile, Auth tokens).
* **Component Store**: For **local UI state** (a complex grid's pagination, or a specific multi-step form) that should be destroyed when the component is destroyed.

---

### 12. Router Store
Syncing the Router into NgRx allows you to:
1.  Include the current URL/Params in your **Selectors** (e.g., getting a user ID directly from the URL).
2.  Time-travel through route changes in the DevTools.

---

### 13. Meta-Reducers
A Meta-Reducer is a "Reducer of Reducers."
* **Common Use Case**: **State Persistance**. You can write a meta-reducer that intercepts every action and saves the new state to `localStorage`, or clears the state on `LOGOUT_ACTION`.

---

### 14. Handling Loading/Error States
I prefer modeling these as part of the state slice for that specific feature:
```typescript
interface UserState {
  users: User[];
  loading: boolean; // boolean flag
  error: string | null; // error message or object
}
```
This allows the UI to show spinners or error alerts based on the current state.

---

### 15. Testing: `provideMockStore`
In a unit test, you use `provideMockStore({ initialState })` in the `TestBed`. You can then use `store.overrideSelector(selector, mockValue)` to force the component to see a specific state without actually setting up reducers or actions.