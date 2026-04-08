## 1. Describe differences between localStorage, sessionStorage, and cookies.

### 1. LocalStorage
Think of this as the "hard drive" of the browser. It is designed for long-term storage of data that doesn't need to be sent to the server with every request.

Persistence: Data persists even after the browser is closed and reopened. It stays there until explicitly cleared via JavaScript or by the user clearing their cache.

Capacity: Roughly 5MB – 10MB (depending on the browser).

Scope: Scoped to the "Origin" (Protocol + Domain + Port).

Usage: Best for user preferences, themes (dark mode), or offline data.

### 2. SessionStorage
This is the "RAM" of the browser. It is nearly identical to LocalStorage in terms of API, but its lifetime is strictly tied to the browser tab.

Persistence: Data is cleared as soon as the specific tab or window is closed. Refreshing the page does not clear it, but opening the same URL in a new tab creates a fresh, empty session.

Capacity: Roughly 5MB.

Scope: Scoped to the specific tab/window.

Usage: Best for sensitive one-time data, like a multi-step form or a temporary state that shouldn't leak to other tabs.

### 3. Cookies
Cookies are the oldest method and are primarily used for server-side communication. Unlike the "Storage" APIs, cookies are sent back and forth between the client and server with every HTTP request.

Persistence: Highly configurable. You can set an Expires or Max-Age attribute. If none is set, it's a "Session Cookie" and dies when the browser closes.

Capacity: Very small—usually 4KB per cookie.

Performance: Since they are sent with every header, large cookies can slow down your web requests.

Usage: Best for Authentication tokens (JWTs) and session IDs.

| Feature | LocalStorage | SessionStorage | Cookies |
| :--- | :--- | :--- | :--- |
| **Expiry** | Never (manual clear) | On Tab Close | Set manually |
| **Capacity** | ~5MB - 10MB | ~5MB | ~4KB |
| **Accessible by Server?** | No | No | **Yes** (sent via headers) |
| **API** | `localStorage.getItem()` | `sessionStorage.getItem()` | `document.cookie` (messy) |
| **Security** | Vulnerable to XSS | Vulnerable to XSS | Can be protected with `HttpOnly` |

### Which one should you use?
- Auth Tokens (JWTs): Use Cookies with the HttpOnly and SameSite flags to prevent theft.

- User Theme (Dark/Light): Use LocalStorage so it persists when they return.

- Single-use form data: Use SessionStorage to ensure it wipes if they close the tab.

- Offline App Data: Use IndexedDB.