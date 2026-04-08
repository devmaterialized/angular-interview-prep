## 1. Describe differences between localStorage, sessionStorage, and cookies.

**Direct Answer:** LocalStorage and SessionStorage are modern Web Storage APIs for client-side data, differing primarily in persistence. Cookies are a legacy storage mechanism designed for server-side communication and authentication.

**Deeper Dive:** For senior developers, the key differentiators are security and performance. Cookies support `HttpOnly` and `SameSite` flags, making them more resilient to XSS and CSRF. Web Storage is synchronous and can block the main thread if overused, whereas cookies have a tiny 4KB limit and are automatically sent with every HTTP request, potentially bloating headers.

**Analogy:** **LocalStorage** is like a **Personal Filing Cabinet** in your office (stays until you trash it). **SessionStorage** is like a **Visitor's Badge** (valid only for one visit/tab). **Cookies** are like **Certified Mail Receipts**—they travel back and forth with every package (HTTP request) to prove your identity.

**Code Example:**
```javascript
// Secure Cookie
document.cookie = "session_id=abc; Secure; HttpOnly; SameSite=Strict";

// Web Storage
localStorage.setItem('user_theme', 'dark');
```

| Feature | LocalStorage | SessionStorage | Cookies |
| :--- | :--- | :--- | :--- |
| **Expiry** | Never (manual clear) | On Tab Close | Set manually |
| **Capacity** | ~5MB - 10MB | ~5MB | ~4KB |
| **Accessible by Server?** | No | No | **Yes** (sent via headers) |
| **API** | `localStorage.getItem()` | `sessionStorage.getItem()` | `document.cookie` |
| **Security** | Vulnerable to XSS | Vulnerable to XSS | Can be protected with `HttpOnly` |

---

## 2. Prototypal Inheritance: How does JS handle objects?

**Direct Answer:** JavaScript uses a prototype-based inheritance model where objects "inherit" properties from other objects via a prototype chain, ending at `null`.

**Deeper Dive:** Understanding the link between `__proto__` and `.prototype` is vital. In modern JS, while we use `class` syntax, it remains "syntactic sugar" over prototypes. Use `Object.create(null)` for "pure" dictionary objects without default inherited properties like `toString`.

**Analogy:** Think of a **Corporate Template**. The "Base Policy" (prototype) defines standard procedures. If a "Regional Office" (instance) doesn't have a specific rule, they look up the chain to the Base Policy. They don't copy the policy; they just refer to it.

**Code Example:**
```javascript
const animal = { eats: true };
const rabbit = Object.create(animal);
console.log(rabbit.eats); // true (inherited)
```

---

## 3. The Event Loop: How does JS handle concurrency?

**Direct Answer:** JavaScript is single-threaded but achieves non-blocking concurrency via the Event Loop, which coordinates the execution of code, events, and queued sub-tasks.

**Deeper Dive:** A senior must distinguish between **Macrotasks** (setTimeout) and **Microtasks** (Promises). Microtasks are processed immediately after the current script and before the next Macrotask.

**Analogy:** Think of a **Bank Teller** (the single thread). **Macrotasks** are customers in the main lobby line. **Microtasks** are "Quick Questions" from the person currently at the window. The teller won't call the next person from the lobby until all "Quick Questions" are answered.

**Code Example:**
```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Output: 1, 4, 3, 2
```

---

## 4. Closures: What are they and why do they matter?

**Direct Answer:** A closure is a function combined with its lexical environment, allowing it to "remember" variables from its outer scope even after the outer function has finished.

**Deeper Dive:** Closures enable data privacy (encapsulation) and are key to patterns like Currying. However, they can lead to memory leaks if they capture large variables that are no longer needed but cannot be garbage collected.

**Analogy:** A closure is like a **Power of Attorney**. Even after the "Principal" (the outer function) has retired, the "Agent" (the inner function) still has the legal right to access the Principal's private bank accounts (variables).

**Code Example:**
```javascript
function createVault() {
  let secret = "1234";
  return () => console.log(secret);
}
const access = createVault();
access(); // "1234"
```
