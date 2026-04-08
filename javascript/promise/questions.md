## 1. What are Promise prototype methods?

Promise prototype methods:
- then (handles the successful resolution of the promise)
- catch (handles the rejection of the promise)
- finally (executes regardless of the promise's outcome)

## 2. What are Promise static methods?

Promise static methods:
- Promise.resolve (creates a resolved promise)
- Promise.reject (creates a rejected promise)

## 3. What are Promise states?

Promise states:
- Pending (initial state, not yet fulfilled or rejected)
- Fulfilled (operation completed successfully)
- Rejected (operation failed)

## 4. Can we join Promises?
Yes, we can join Promises using Promise.all() or Promise.race().

- Promise.all()
Fulfills when all of the promises fulfill; rejects when any of the promises rejects.

- Promise.allSettled()
Fulfills when all promises settle.

- Promise.any()
Fulfills when any of the promises fulfills; rejects when all of the promises reject.

- Promise.race()
Settles when any of the promises settles. In other words, fulfills when any of the promises fulfills; rejects when any of the promises rejects.

