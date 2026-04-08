## 1. Observable vs. Promise: What is the fundamental difference, and why does Angular favor Observables for HTTP requests?

A Promise handles a **single event** (one value, then it's done) and it starts immediately.
An Observable is a **stream** that can emit multiple values over time and is lazy—it doesn't start until you subscribe. Angular favors Observables for HTTP because they allow for cancellation (aborting a request) and better handling of retry logic or multiple emissions.

## 2. The async Pipe: Why is it considered best practice compared to manual .subscribe() calls in a component?

It is best practice because it automatically manages the subscription lifecycle. 
It subscribes when the component initializes and, more importantly, unsubscribes when the component is destroyed, preventing memory leaks. It also triggers Change Detection automatically when new values arrive, and displays the current value of the observable.

## 3. map vs. tap: One transforms data, the other is for 'side effects.' Can you explain the difference?

map: Transforms the data. It takes an input, modifies it, and passes the new result down the pipe (e.g., changing a string to uppercase).

tap: Performs a side effect. It "taps" into the stream to execute code (like console.log or updating a variable) without changing the value or state of the stream itself.

## 4. Unsubscribing: Why is it dangerous to leave subscriptions open, and what are two ways to ensure they are closed?

Leaving subscriptions open causes memory leaks and "ghost" logic where code continues to run in the background for components that no longer exist.

takeUntil(): Use a subject that emits when the component is destroyed to kill the stream.

``` javascript
import { interval, fromEvent } from 'rxjs';
import { takeUntil } from 'rxjs/operators';

// 1. The source: emits every 1 second
const source$ = interval(1000);

// 2. The notifier: emits when the 'stop-button' is clicked
const stopBtn = document.getElementById('stop-button');
const stop$ = fromEvent(stopBtn, 'click');

// 3. Applying takeUntil
const managedSource$ = source$.pipe(
  takeUntil(stop$)
);

managedSource$.subscribe({
  next: (val) => console.log('Counter:', val),
  complete: () => console.log('Stream completed!')
});
```

In this scenario, we have a counter that runs every second, but we want it to stop the moment a specific button is clicked.

Async Pipe: Let Angular handle the teardown automatically.

## 5. of vs. from: If I want to turn an array into a stream of individual items, which one do I use?
Use from.

of([1, 2, 3]) emits the entire array as one single item.

from([1, 2, 3]) flattens the array and emits 1, then 2, then 3 individually.

## 6. filter operator: How would you prevent a stream from emitting if the value is null or undefined?
You use a predicate function to check for "truthy" values:

``` typescript
filter(value => value !== null && value !== undefined)
```

## 7. Cold vs. Hot Observables: In plain English, what is the difference?

Cold: Like a Netflix movie. The data starts from the beginning for every individual subscriber (e.g., an HTTP request). It produces the value inside the observer.

Hot: Like a Live Concert. The data is produced regardless of subscribers; if you join late, you missed the beginning (e.g., mouse clicks or a shared state). The data is being produced outside the observer.

## 8. The Big Four Mapping Operators: Can you explain the use cases for switchMap, mergeMap, concatMap, and exhaustMap?

Choosing between these four "Transformation" operators is all about how you want to handle concurrency. In RxJS, these are used when your main stream triggers another stream (like an HTTP request).

Think of it this way: what should happen if a new request comes in while the old one is still running?

### switchMap (The "Switch to Latest")
Behavior: If a new emission occurs, it immediately cancels (unsubscribes from) the previous inner Observable and starts the new one.

Best for: Search-as-you-type or any "get the latest data" scenario.

Analogy: You are ordering coffee. If you change your mind and shout a new order before the barista finishes the first one, they throw the old cup away and start the new one.

### mergeMap (The "Parallel Processor")
Behavior: It allows multiple inner Observables to run concurrently. It doesn't care about order; it just emits values as soon as they arrive.

Best for: Scenarios where you want maximum speed and don't care about the sequence (e.g., deleting multiple items from a list independently).

Analogy: A busy fast-food kitchen. Orders come in, multiple chefs cook at once, and whoever finishes first hands out the food.

### concatMap (The "Queue Master")
Behavior: It waits for the current inner Observable to complete before starting the next one. It preserves the order of emissions.

Best for: Operations that must happen in sequence, like a series of database updates where Step B depends on Step A.

Analogy: A single-file line at a bank. Even if five people arrive, the teller only helps the second person once the first person is completely finished.

### exhaustMap (The "Ignore While Busy")
Behavior: It ignores all new incoming emissions while the current inner Observable is still running. It only listens again once the current task is finished.

Best for: Login buttons or "Submit" clicks to prevent double-submissions.

Analogy: An elevator. Once the doors close and it starts moving, it ignores people pressing the button in the hallway until it reaches its floor and becomes idle again.

## 9. Subject Varieties: What is the difference between a Subject, BehaviorSubject, and ReplaySubject?
Subject: A standard "multicaster." New subscribers only get values emitted after they subscribe.

BehaviorSubject: Requires an initial value. New subscribers immediately get the current/latest value.

ReplaySubject: "Replays" a specific number of previous emissions to new subscribers, even if they missed them originally.
## 10. Error Handling: How do catchError and retry work? If an error occurs, does the stream stay 'alive'?
catchError: Intercepts an error and returns a "fallback" observable to keep the flow going.

retry: Automatically resubscribes to the source if it fails.

Does it stay alive? Usually, no. An error is a "terminal" event. Once an observable errors out, it dies. catchError works by replacing the dead stream with a new one.
## 11. forkJoin vs. combineLatest: You need to wait for three API calls to finish before showing a page. Which one do you use and why?
Use forkJoin. It acts like Promise.all(); it waits for all observables to complete and then emits their final values as an array. combineLatest would emit every time any of the three changed, which is overkill for a one-time page load.
## 12. shareReplay: Why would you use this operator for an HTTP request that multiple components are interested in?
If you have one HTTP request but five components subscribing to it, shareReplay ensures the request is made only once. It caches the result and "replays" it to any late subscribers, preventing redundant network calls.
## 13. Debouncing vs. Throttling: If you're building a search-as-you-type feature, which one do you use to save your backend?
Use debounceTime. It waits for a "pause" in typing (e.g., 300ms) before sending the request. Throttling would send requests at a steady interval regardless of pauses, which is less efficient for search.
## 14. distinctUntilChanged: Why is this operator crucial when dealing with state updates or search inputs?
It prevents the stream from emitting if the new value is identical to the previous one. This is crucial for performance; for example, if a user types "search," hits backspace, then types "h" again, you don't want to trigger a new API call for the same word.
