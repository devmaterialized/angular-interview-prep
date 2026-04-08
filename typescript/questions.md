### 1. Static vs. Dynamic Typing: What is the main problem TypeScript solves?

**Direct Answer:** TypeScript introduces a static type system over JavaScript, allowing developers to define and enforce data contracts at compile-time rather than runtime. This drastically reduces "undefined is not a function" errors.

**Deeper Dive:** In large enterprise systems, the cost of a runtime error is significantly higher than the cost of development-time type checking. TypeScript's "Soundness" isn't 100% (due to `any`, non-null assertions, etc.), but it enables "Local Reasoning"—the ability to understand a function's behavior just by looking at its signature without tracing the entire execution flow.

**Analogy:** Think of static typing like a **Compliance Audit** in Fintech. In a dynamically typed system (JS), you find out the transaction failed only when the money reaches the ledger and doesn't match the format. In a statically typed system (TS), the transaction is validated against the regulatory schema *before* it even enters the pipeline.

**Code Example:**
```typescript
interface Transaction {
  id: string;
  amount: number;
}

function processPayment(tx: Transaction) {
  // TS ensures tx.amount is always a number here
  return tx.amount.toFixed(2);
}
```

---

### 2. Interface vs. Type Alias: When should you use which?

**Direct Answer:** Interfaces are best for defining object shapes and support **declaration merging**, while Type Aliases are more versatile, supporting unions, intersections, and primitives.

**Deeper Dive:** From a performance perspective, TypeScript caches interface lookups more efficiently than type aliases. However, types are indispensable for "Type Algebra" (e.g., `type Status = 'Pending' | 'Approved' | 'Rejected'`). In modern TS, the gap is narrowing, but the rule of thumb is: use `interface` until you need a `type`.

**Analogy:** An **Interface** is like a standard insurance policy template. Different departments can add "riders" (declaration merging) to it. A **Type Alias** is like a custom-tailored financial instrument—it might be a mix of various assets (union/intersection) that can't be modified once the contract is signed.

**Code Example:**
```typescript
// Interface: Extendable
interface User { name: string; }
interface User { id: number; } // Merged!

// Type: Flexible
type ID = string | number;
type Policy = { id: ID } & { premium: number };
```

---

### 3. The `any` vs. `unknown` Types: Which is safer?

**Direct Answer:** `any` completely opts out of type checking, while `unknown` is the type-safe counterpart that requires a type check (narrowing) before any operations can be performed on it.

**Deeper Dive:** Using `any` spreads "type rot" through your system because it can be assigned to anything and anything can be assigned to it. `unknown` forces you to perform "Type Guarding," ensuring that your code is resilient against unexpected data shapes at the boundaries of your application.

**Analogy:** `any` is like a **Blank Check**—it can represent any value and bypasses all security protocols. `unknown` is like an **Encrypted Data Packet**—you know it contains something valuable, but you cannot process it until you provide the correct decryption key (Type Guard).

**Code Example:**
```typescript
function handleInput(val: unknown) {
  if (typeof val === 'string') {
    console.log(val.toUpperCase()); // Safe
  }
}
```

---

### 4. Generics: How do they create reusable code?

**Direct Answer:** Generics are "type variables" that allow you to capture the type of an argument and use it to enforce constraints elsewhere in the function or class, enabling the creation of highly reusable components.

**Deeper Dive:** Advanced generics use `extends` to constrain the types allowed. For seniors, understanding "Generic Parameter Defaults" and "Generic Constraints" is crucial for building libraries or complex data structures like `Repository<T>`.

**Analogy:** Generics are like a **Standardized Shipping Container**. The container itself doesn't care if it's carrying electronics or grain (T), but it ensures that the handling equipment (the functions) can interact with it safely regardless of the contents, as long as they fit the container's specs.

**Code Example:**
```typescript
interface ApiResponse<T> {
  data: T;
  status: number;
}

const userResponse: ApiResponse<{ name: string }> = {
  data: { name: 'Alice' },
  status: 200
};
```

---

### 5. Union and Intersection Types: How do you combine types?

**Direct Answer:** Union types (`|`) represent an "OR" relationship, allowing a value to be one of several types. Intersection types (`&`) represent an "AND" relationship, merging multiple types into one.

**Deeper Dive:** Unions are the foundation of **Discriminated Unions**, a powerful pattern for handling state. Intersections are often used for "Mixins" or extending existing types without the overhead of class inheritance.

**Analogy:** A **Union** is like a multi-currency account: it can hold USD *or* EUR. An **Intersection** is like a "Credit-Linked Note"—it is simultaneously a debt instrument *and* a derivative contract, possessing properties of both.

**Code Example:**
```typescript
type Result = { success: true; data: string } | { success: false; error: Error };

function process(res: Result) {
  if (res.success) {
    console.log(res.data); // Narrowed to success object
  }
}
```

---

### 6. Type Inference: Do you have to type everything?

**Direct Answer:** No! TypeScript is very smart at **inferring** types based on the assigned value. You should only explicitly type variables when the compiler can't figure it out or when you want to enforce a specific contract.

**Deeper Dive:** Excessive explicit typing (especially on local variables) can make code verbose and harder to maintain. However, explicit return types on public functions are recommended to prevent accidental changes to your API contract.

**Analogy:** It's like **Automated Fraud Detection**. The system "infers" the risk level of a transaction based on historical patterns (the assigned value) without you having to manually flag every single one. You only step in and manually "type" it if the situation is ambiguous.

**Code Example:**
```typescript
let x = 'hello'; // TypeScript infers 'string'
// x = 123; // Error!
```

---

### 7. Enums vs. Const Enums: What happens at runtime?

**Direct Answer:** Standard `enum` generates a real JavaScript object at runtime, allowing for reverse mapping. `const enum` is completely erased during compilation, and usage is replaced with hardcoded values.

**Deeper Dive:** Use `const enum` when you want to minimize bundle size and performance overhead. Use `enum` if you need to iterate over the keys at runtime or if you need the reverse mapping feature (value to key).

**Analogy:** A standard **Enum** is like a **Company Directory**—it exists as a physical list you can look up. A **Const Enum** is like a **Secret Code**—everyone has memorized that "Code A" means "Buy," so the list doesn't need to be printed on every contract.

**Code Example:**
```typescript
const enum Status { Pending, Approved }
const s = Status.Pending; // Compiled to: const s = 0;
```

---

### 8. Type Guards and Narrowing: How do you handle uncertainty?

**Direct Answer:** Type narrowing is the process of moving from a broad type to a more specific one using runtime checks like `typeof`, `instanceof`, the `in` operator, or custom **Type Predicates**.

**Deeper Dive:** For complex domain models, **User-Defined Type Guards** (`val is MyType`) are essential. They allow you to encapsulate complex validation logic into a reusable function that the TypeScript compiler trusts.

**Analogy:** It’s like a **Know Your Customer (KYC)** process. You start with a "Potential Customer" (broad type). After checking their ID and address (Type Guard), you "narrow" them to a "Verified Premium Client" who is allowed to perform specific transactions.

**Code Example:**
```typescript
function isPremiumMember(user: User | PremiumUser): user is PremiumUser {
  return (user as PremiumUser).tier === 'Gold';
}
```

---

### 9. Structural Typing: What is "Duck Typing"?

**Direct Answer:** TypeScript is **Structurally Typed**: if two objects have the same shape, they are considered the same type, regardless of their names or explicit inheritance.

**Deeper Dive:** While structural typing is flexible, it can sometimes lead to "Accidental Compatibility." Seniors use **Branding** (or Tagging) to simulate nominal typing when they need to distinguish between two types with the same structure (e.g., `USDAmount` vs `EURAmount`).

**Analogy:** In a structurally typed world, if a document has a "Name," "SSN," and "Address" field, it's a "Tax Form," regardless of whether it was printed by the government or by you. "If it walks like a duck and quacks like a duck, it's a duck."

**Code Example:**
```typescript
interface Point { x: number; y: number; }
function logPoint(p: Point) { console.log(p.x, p.y); }

const myObj = { x: 1, y: 2, z: 3 };
logPoint(myObj); // Works! Shape matches.
```

---

### 10. `keyof` and `typeof` Operators: How do they work in a type context?

**Direct Answer:** `typeof` extracts the type of an existing variable, while `keyof` produces a union of keys from an object type.

**Deeper Dive:** Combining these operators allows you to create dynamic, type-safe maps or lookups that automatically stay in sync with your runtime objects. This is powerful for creating generic "Getter" functions.

**Analogy:** `typeof` is like **Carbon-Dating** an artifact to find out what it's made of. `keyof` is like looking at the **Index of a Book** to see all the available chapters you can read.

**Code Example:**
```typescript
const settings = { theme: 'dark', volume: 10 };
type Settings = typeof settings;
type SettingKey = keyof Settings; // 'theme' | 'volume'
```

---

### 11. The `satisfies` Operator: Why use it over type annotations?

**Direct Answer:** `satisfies` validates that an expression matches a type without changing the resulting type of that expression, providing both type safety and the most specific type inference possible.

**Deeper Dive:** When you use a type annotation (`const x: Type = ...`), you lose the specificity of the value. When you use `satisfies`, TS ensures the value matches the contract but keeps literal types intact.

**Analogy:** `satisfies` is like a **Quality Control Checkpoint** on a factory line. The product is checked to ensure it meets "Safety Standards" (the type), but it doesn't lose its "Unique Serial Number" (specific type) in the process.

**Code Example:**
```typescript
type Config = Record<string, string | number>;
const config = { host: "localhost", port: 8080 } satisfies Config;
config.host.toUpperCase(); // Safe! TS knows host is specifically a string.
```

---

### 12. Conditional Types: How to build "logic" into your types?

**Direct Answer:** Conditional types choose a type based on a condition (`T extends U ? X : Y`), enabling "Type-Level Programming."

**Deeper Dive:** Combined with the `infer` keyword, conditional types can "reach inside" other types to extract information (e.g., getting the return type of a function).

**Analogy:** This is the **Logic Engine** of a dynamic pricing model. `type Price<T> = T extends 'Premium' ? 100 : 50`. The system automatically decides which "Type" of price to apply based on the "Type" of user.

**Code Example:**
```typescript
type Flatten<T> = T extends any[] ? T[number] : T;
type Str = Flatten<string[]>; // string
```
