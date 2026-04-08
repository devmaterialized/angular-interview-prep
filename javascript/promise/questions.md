## 1. What are Promise prototype methods?

**Direct Answer:** The primary prototype methods are `.then()`, `.catch()`, and `.finally()`. They allow you to chain logic based on the outcome of a promise.

**Deeper Dive:** `.then()` can take two arguments (success and failure handlers), but using `.catch()` is preferred for readability. `.finally()` is essential for resource cleanup (e.g., hiding a loader) because it runs regardless of the result. Note that `.then()` always returns a new promise, enabling chaining.

**Analogy:** It's like a **Legal Settlement Process**. `.then()` is what happens if the deal is signed; `.catch()` is the litigation if it fails; `.finally()` is paying the court fees regardless of who won.

**Code Example:**
```javascript
fetchData()
  .then(res => res.json())
  .catch(err => console.error(err))
  .finally(() => stopSpinner());
```

---

## 2. What are Promise static methods?

**Direct Answer:** Static methods like `Promise.resolve(value)` and `Promise.reject(reason)` allow you to create pre-settled promises immediately.

**Deeper Dive:** These are incredibly useful for wrapping non-promise values into a promise-based API or for unit testing. In modern JS, we also have `Promise.all`, `Promise.allSettled`, `Promise.any`, and `Promise.race` for managing multiple concurrent operations.

**Analogy:** `Promise.resolve` is like a **Certified Check**—the funds are already verified and guaranteed before you even take it to the bank.

**Code Example:**
```javascript
const preApproved = Promise.resolve({ status: 'Approved' });
```

---

## 3. What are Promise states?

**Direct Answer:** A Promise is always in one of three states: **Pending** (initial), **Fulfilled** (success), or **Rejected** (failure).

**Deeper Dive:** Once a promise moves from Pending to either Fulfilled or Rejected, it is "Settled" and its state can never change again. This immutability is what makes promises more predictable than callbacks for asynchronous flow control.

**Analogy:** A Promise is like a **Stock Options Contract**. When you receive the contract, it's **Pending**. Eventually, it's either "Exercised" (**Fulfilled**) and you get the shares, or it "Expires Worthless" (**Rejected**).

**Code Example:**
```javascript
const p = new Promise((res) => setTimeout(() => res('Done'), 100));
// Starts as Pending, ends as Fulfilled
```

---

## 4. Can we join Promises?

**Direct Answer:** Yes, using methods like `Promise.all()`, `Promise.allSettled()`, `Promise.any()`, and `Promise.race()`.

**Deeper Dive:**
- `all`: All or nothing. Best for dependent batch calls.
- `allSettled`: Waits for everyone, regardless of success. Best for independent cleanup.
- `any`: First to succeed. Best for redundant API calls.
- `race`: First to settle (win or lose). Best for timeouts.

**Analogy:** `Promise.all` is like a **Syndicated Loan**. If one bank in the syndicate fails to fund, the whole deal falls through.

**Code Example:**
```javascript
// Adding a timeout to a fetch
Promise.race([
  fetch('/api'),
  new Promise((_, rej) => setTimeout(() => rej('Timeout'), 5000))
]);
```
