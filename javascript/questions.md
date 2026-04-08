## 1. Describe differences between localStorage, sessionStorage, and cookies.

**Direct Answer:** LocalStorage and SessionStorage are modern Web Storage APIs for client-side data, differing primarily in persistence (permanent vs. tab-session). Cookies are a legacy storage mechanism primarily designed for server-side communication and authentication.

**Deeper Dive:** For senior developers, security and performance are the key differentiators. Cookies support `HttpOnly` and `SameSite` flags, making them more resilient to XSS (Cross-Site Scripting) and CSRF (Cross-Site Request Forgery) when handling JWTs. Web Storage is synchronous and can block the main thread if overused, whereas cookies have a tiny 4KB limit and are automatically sent with every HTTP request, potentially bloating headers.

**Analogy:** **LocalStorage** is like a **Personal Filing Cabinet** in your office (stays there until you trash it). **SessionStorage** is like a **Visitor's Badge** (valid only for the duration of one visit/tab). **Cookies** are like **Certified Mail Receipts**—they travel back and forth between you and the post office (server) every time a package is sent to prove your identity.

**Code Example:**
```javascript
// Secure Cookie Setup
document.cookie = "session_id=abc; Secure; HttpOnly; SameSite=Strict";

// Web Storage API
localStorage.setItem('user_theme', 'dark');
```

---

## 2. Prototypal Inheritance: How does JS handle objects?

**Direct Answer:** JavaScript uses a prototype-based inheritance model where objects "inherit" properties and methods from other objects via a prototype chain. Every object has an internal link (`[[Prototype]]`) to another object until it reaches `null`.

**Deeper Dive:** Understanding the difference between `__proto__` (the instance link) and `.prototype` (the constructor's blueprint) is vital. In modern JS, while we use `class` syntax, it remains "syntactic sugar" over prototypes. Seniors should be aware of the performance implications of long prototype chains and the use of `Object.create(null)` to create "pure" dictionary objects without default properties like `toString`.

**Analogy:** Think of a **Corporate Template**. The "Base Policy" (prototype) defines standard procedures. If a specific "Regional Office" (instance) doesn't have a specific rule for a situation, they look up the chain to the Base Policy. They don't copy the whole policy; they just refer to it when needed.

**Code Example:**
```javascript
const animal = { eats: true };
const rabbit = Object.create(animal);
console.log(rabbit.eats); // true (inherited)
```

---

## 3. The Event Loop: How does JS handle concurrency?

**Direct Answer:** JavaScript is single-threaded, but it achieves non-blocking concurrency via the Event Loop. It coordinates the execution of code, collecting and processing events, and executing queued sub-tasks.

**Deeper Dive:** A senior must distinguish between **Macrotasks** (setTimeout, setInterval, I/O) and **Microtasks** (Promises, MutationObserver). Microtasks are always processed immediately after the current script and before the next Macrotask. This is why a resolved `Promise.then()` will always execute before a `setTimeout(0)`.

**Analogy:** Think of a **Bank Teller** (the single thread). The teller handles one customer at a time. **Macrotasks** are customers waiting in the main lobby line. **Microtasks** are "Quick Questions" from the person currently at the window. The teller won't call the next person from the lobby until all "Quick Questions" from the current transaction are answered.

**Code Example:**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0); // Macrotask
Promise.resolve().then(() => console.log('3')); // Microtask
console.log('4');
// Output: 1, 4, 3, 2
```

---

## 4. Closures: What are they and why do they matter?

**Direct Answer:** A closure is the combination of a function and the lexical environment within which that function was declared. It allows a function to "remember" and access variables from its outer scope even after the outer function has finished executing.

**Deeper Dive:** Closures are the primary mechanism for data privacy in JS (encapsulation) and are heavily used in functional programming patterns like Currying and Memoization. However, seniors must be cautious of memory leaks, as closures prevent the garbage collector from cleaning up variables that are still "captured."

**Analogy:** A closure is like a **Power of Attorney**. Even after the "Principal" (the outer function) has left the country or retired, the "Agent" (the inner function) still has the legal right to access the Principal's private bank accounts (variables).

**Code Example:**
```javascript
function createVault() {
  let secret = "1234";
  return () => console.log(secret); // Closure captures 'secret'
}
const access = createVault();
access(); // "1234"
```

---

## 5. `this` Keyword: How is its value determined?

**Direct Answer:** The value of `this` is determined by *how* a function is called, not where it is defined. It generally refers to the "owner" of the function call.

**Deeper Dive:** There are four rules:
1. **Implicit Binding:** The object before the dot (e.g., `obj.fn()`).
2. **Explicit Binding:** Using `.call()`, `.apply()`, or `.bind()`.
3. **New Binding:** Inside a constructor function or class.
4. **Default Binding:** In non-strict mode, `window`/`global`; in strict mode, `undefined`.
*Crucially*, **Arrow Functions** do not have their own `this`; they inherit it from their enclosing lexical scope.

**Analogy:** `this` is like the word **"The Client"** in a legal contract. Its meaning depends entirely on which contract (context) you are currently reading. If you move the "Contract Clause" (function) to a different "Agreement" (object), "The Client" now refers to the new party.

**Code Example:**
```javascript
const obj = {
  name: 'FinanceApp',
  printName: function() { console.log(this.name); },
  printArrow: () => console.log(this.name)
};

obj.printName(); // 'FinanceApp'
obj.printArrow(); // undefined (or window.name)
```
