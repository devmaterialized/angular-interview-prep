## 1. List change detection strategies and when to use each one.
### Default Strategy (ChangeDetectionStrategy.Default)
By default, Angular uses the CheckAlways strategy. This means the framework is a bit "paranoid"—it checks the component and its children whenever anything happens in the app (clicks, timeouts, XHR requests).

When to use: Small, simple applications where performance isn't a bottleneck.
Components with highly dynamic internal state that doesn't rely strictly on @Input() properties.

When you're prototyping and don't want to worry about manual UI updates yet.

### OnPush Strategy (ChangeDetectionStrategy.OnPush)
This tells Angular: "Don't check me unless I tell you to, or my inputs change." It treats the component as more of a pure function.

When to use:

Performance-critical apps: This is the gold standard for large-scale enterprise applications.

Dumb/Presentational components: Components that just take data and render it.

Immutable Data: When you are using Immutable.js or ensuring you pass new object references rather than mutating existing ones.

Observable-heavy logic: When you are using the async pipe, which plays beautifully with OnPush.

