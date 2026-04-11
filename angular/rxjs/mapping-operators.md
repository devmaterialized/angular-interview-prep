In the world of RxJS, **mapping operators** (also known as Transformation Operators) are essential for handling "Higher-Order Observables"—which is just a fancy way of saying an Observable that emits other Observables (like an HTTP request triggered by a button click).

Here is the breakdown of the "Big Four" and how they handle concurrent inner Observables.

---

## 1. switchMap: The "Reset" Button
`switchMap` is the most common mapping operator. When a new value arrives, it **unsubscribes** from the previous inner Observable and starts a new one. It only cares about the latest emission.

* **Best for:** Search-as-you-type, cancelling obsolete HTTP requests.
* **Behavior:** "Switch to the new one, kill the old one."

```typescript
import { fromEvent, switchMap, timer } from 'rxjs';

// Every click starts a 2-second timer. 
// If you click again before 2 seconds, the previous timer is cancelled.
const clicks = fromEvent(document, 'click');
const result = clicks.pipe(
  switchMap(() => timer(2000))
);

result.subscribe(val => console.log('Timer finished!'));
```

---

## 2. mergeMap: The "Parallel" Worker
`mergeMap` (formerly `flatMap`) allows multiple inner Observables to run concurrently. It doesn't care about order or timing; it just flattens everything as it happens.

* **Best for:** Requests that should happen independently and fast (e.g., deleting multiple items in a list).
* **Behavior:** "Do everything at once as fast as possible."

```typescript
import { of, mergeMap, delay } from 'rxjs';

// Emissions happen in parallel. 
// Even though 'A' starts first, 'B' finishes first because its delay is shorter.
of('A', 'B').pipe(
  mergeMap(val => val === 'A' ? of(val).pipe(delay(2000)) : of(val).pipe(delay(1000)))
).subscribe(console.log); 

// Output: B, then A
```

---

## 3. concatMap: The "Queue" Manager
`concatMap` waits for the current inner Observable to complete before starting the next one. It preserves the order of emissions by queuing them up.

* **Best for:** Operations where order matters (e.g., sequential database updates).
* **Behavior:** "Wait your turn in line."

```typescript
import { of, concatMap, delay } from 'rxjs';

// Even though 'B' is faster, it must wait for 'A' to finish its 2s delay.
of('A', 'B').pipe(
  concatMap(val => val === 'A' ? of(val).pipe(delay(2000)) : of(val).pipe(delay(1000)))
).subscribe(console.log);

// Output: A (after 2s), then B (after 1s more)
```

---

## 4. exhaustMap: The "Ignore" Specialist
`exhaustMap` ignores all incoming values while the current inner Observable is still running. Once the inner Observable completes, it will accept the next value.

* **Best for:** Login buttons or "Refresh" buttons where you don't want accidental double-submissions.
* **Behavior:** "I'm busy, don't talk to me until I'm done."

```typescript
import { fromEvent, exhaustMap, timer } from 'rxjs';

// If you spam click, only the first click triggers the timer. 
// Subsequent clicks are ignored until the 2 seconds are up.
const clicks = fromEvent(document, 'click');
const result = clicks.pipe(
  exhaustMap(() => timer(2000))
);

result.subscribe(() => console.log('Operation complete, ready for next click!'));
```

---

### Quick Comparison Table

| Operator | Concurrency Strategy | Use Case Example |
| :--- | :--- | :--- |
| **switchMap** | Cancel previous, start new | Typeahead Search |
| **mergeMap** | Run all in parallel | General independent API calls |
| **concatMap** | Queue and run in order | Sequential Save operations |
| **exhaustMap** | Ignore new while busy | Submit/Login buttons |