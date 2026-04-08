### 1. Static vs. Dynamic Typing: What is the main problem TypeScript solves?
JavaScript is **dynamically typed**, meaning variables can change types at runtime (a string can suddenly become a number), often leading to "undefined is not a function" errors. TypeScript introduces **static typing**, where types are checked during development (compile-time). This catches bugs before you even run your code.

---

### 2. Interface vs. Type Alias: When should you use which?
While they are very similar, there are key differences:
* **Interfaces**: Best for defining the "shape" of an object. They support **declaration merging** (if you define the same interface twice, they merge), making them ideal for libraries.
* **Type Aliases**: More flexible. Use them for **unions** (e.g., `type Status = 'open' | 'closed'`), **intersections**, or primitives. They cannot be changed once created.

---

### 3. The `any` vs. `unknown` Types: Which is safer?
* **`any`**: Effectively turns off type checking. It allows you to do anything, which defeats the purpose of TypeScript.
* **`unknown`**: A "type-safe" version of any. You can't perform operations on an `unknown` variable until you **narrow** the type (e.g., using an `if` check or a type guard). Always prefer `unknown` for data coming from APIs.

---

### 4. Generics: How do they create reusable code?
Generics allow you to create components or functions that work over a variety of types rather than a single one.
* **Example**: A function `identity<T>(arg: T): T` ensures that the type returned is exactly the same as the type passed in, without losing the specific type information to `any`.



---

### 5. Union and Intersection Types: How do you combine types?
* **Union (`|`)**: Allows a value to be one of several types (e.g., `string | number`). Think of it as "OR."
* **Intersection (`&`)**: Combines multiple types into one (e.g., `Person & Employee`). The resulting type has all members of all combined types. Think of it as "AND."

---

### 6. Type Inference: Do you have to type everything?
No! TypeScript is very smart at **inferring** types. If you write `let x = 'hello'`, TypeScript automatically knows `x` is a `string`. You should only explicitly type variables when the compiler can't figure it out or when you want to enforce a specific contract.

---

### 7. Enums vs. Const Enums: What happens at runtime?
* **`enum`**: Generates a real JavaScript object at runtime, which allows for reverse mapping (looking up a key by its value).
* **`const enum`**: Is completely erased during compilation. TypeScript replaces usage of the enum with the hardcoded values, which results in smaller, faster JavaScript code.

---

### 8. Type Guards and Narrowing: How do you handle uncertainty?
Type narrowing is the process of moving from a less specific type to a more specific one.
* **Methods**: You can use `typeof`, `instanceof`, or custom **Type Predicates** (`value is Type`). This tells the compiler: "Inside this `if` block, I've verified that this variable is definitely a string."

---

### 9. Structural Typing: What is "Duck Typing"?
TypeScript uses a **structural type system**. This means that if two objects have the same properties with the same types, they are considered compatible, regardless of their names or where they came from. 
> "If it walks like a duck and quacks like a duck, it’s a duck."

---

### 10. `keyof` and `typeof` Operators: How do they work in a type context?
* **`typeof`**: In a type context, it extracts the type of an existing variable or object.
* **`keyof`**: Takes an object type and produces a string or numeric literal union of its keys.
* **Combo**: `type Props = keyof typeof myObject` creates a union of all keys found in the `myObject` variable.

