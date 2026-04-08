## 1. Observables vs. Promises: The "Streaming" Mindset

**Direct Answer:** Promises are for single, eager, non-cancelable asynchronous events. Observables are for multiple, lazy, cancelable streams of data over time.

**Deeper Dive:** For a senior, the key is **Declarative Composition**. Observables allow you to treat asynchronous events as collections that can be transformed, combined, and throttled using operators. Unlike Promises, which execute immediately upon creation, Observables are "blueprints" that only execute when a consumer subscribes, allowing for powerful retry and teardown logic.

**Analogy:** A **Promise** is like a **Bank Wire Transfer**—you initiate it, and it either completes or fails once. An **Observable** is like a **Direct Debit Subscription**—it can pull money (data) periodically, you can pause or cancel it at any time, and it stays active until you "unsubscribe."

**Code Example:**
```typescript
// Observable: Emits every second, can be cancelled
const sub = interval(1000).subscribe(console.log);
setTimeout(() => sub.unsubscribe(), 5000);
```

---

## 2. Higher-Order Mapping Operators: switchMap vs. mergeMap vs. concatMap vs. exhaustMap

**Direct Answer:** These operators handle "Inner Observables" (concurrency).
- **switchMap**: Cancels the previous inner observable (best for search).
- **mergeMap**: Runs all inner observables in parallel (best for independent saves).
- **concatMap**: Queues inner observables (best for sequential updates).
- **exhaustMap**: Ignores new emissions while the inner one is running (best for login buttons).

**Deeper Dive:** The choice depends on the desired **Race Condition** handling. Seniors should understand that `concatMap` is safer for database integrity, while `switchMap` is essential for preventing stale data in UI components.

**Analogy:** Think of an **Insurance Claims Processor**:
- **switchMap**: A customer changes their mind mid-claim. You shred the old paperwork and start fresh with their new request.
- **mergeMap**: Five customers file claims at once. You assign five different adjusters to process them all simultaneously.
- **concatMap**: A queue of claims. You must finish processing the first one before you even look at the second.
- **exhaustMap**: You are busy on the phone. You ignore all other incoming calls until you hang up.

**Code Example:**
```typescript
// Prevents double-submissions by ignoring clicks until the API returns
submitClicks$.pipe(
  exhaustMap(data => this.api.save(data))
).subscribe();
```

---

## 3. Cold vs. Hot Observables & Multicasting

**Direct Answer:** Cold observables create a new producer for every subscriber (e.g., a fresh HTTP call). Hot observables share a single producer among multiple subscribers (e.g., a mouse move event or a Subject).

**Deeper Dive:** Converting a Cold observable to Hot is called **Multicasting**, typically achieved via the `share()` or `shareReplay()` operators. For seniors, `shareReplay(1)` is a standard pattern for caching API responses in services so that multiple components can consume the same data without triggering redundant network requests.

**Analogy:** A **Cold Observable** is like a **Movie on Demand (Netflix)**—it starts from the beginning for everyone whenever they press play. A **Hot Observable** is like a **Live News Broadcast**—it's happening regardless of who is watching; if you tune in late, you missed the beginning.

**Code Example:**
```typescript
// shareReplay(1) caches the last emitted value for future subscribers
readonly data$ = this.http.get('/api/stats').pipe(shareReplay(1));
```

---

## 4. Error Handling: catchError vs. retry vs. retryWhen

**Direct Answer:** `catchError` intercepts a failing stream and allows you to return a "fallback" or re-throw. `retry` simply re-subscribes to the source in hopes of success.

**Deeper Dive:** In enterprise apps, simple `retry` is often dangerous (can DOS your own server). Seniors should implement **Exponential Backoff** using `retry` with a delay or the `retry` operator's configuration object in RxJS 7+. This ensures that we wait longer between each successive retry attempt.

**Analogy:** It’s like a **Payment Gateway**. If a transaction fails, `catchError` is the logic that offers a "Alternative Payment Method." `retry` is the user clicking "Submit" again. **Exponential Backoff** is the system saying "Wait 5 seconds, then 10, then 30" before trying the bank again.

**Code Example:**
```typescript
this.http.get('/api/data').pipe(
  retry({ count: 3, delay: (error, retryCount) => timer(retryCount * 1000) }),
  catchError(err => of(fallbackData))
);
```

---

## 5. Subject Varieties: Subject, BehaviorSubject, ReplaySubject

**Direct Answer:**
- **Subject**: No initial value, no memory.
- **BehaviorSubject**: Has an initial value and "remembers" the last value for new subscribers.
- **ReplaySubject**: Remembers a specific *buffer* of previous values.

**Deeper Dive:** `BehaviorSubject` is the workhorse of state management in Angular services because it ensures that any component subscribing "late" still gets the current state. `AsyncSubject` (rarely used) only emits the last value when the stream completes.

**Analogy:**
- **Subject**: A **Live PA System**. If you aren't in the room when the announcement happens, you missed it.
- **BehaviorSubject**: A **"Last Price" Display** on a trading floor. Even if you just arrived, you can see the current price.
- **ReplaySubject**: A **Meeting Recording**. You can watch the last 15 minutes or the whole thing even if you joined late.

**Code Example:**
```typescript
// Classic State Service Pattern
private userState = new BehaviorSubject<User | null>(null);
user$ = this.userState.asObservable(); // Expose as read-only
```
