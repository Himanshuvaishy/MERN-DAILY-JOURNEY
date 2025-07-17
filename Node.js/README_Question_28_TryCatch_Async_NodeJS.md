# ✅ 28. What is the purpose of `try-catch` in async code?

---

### 🔹 1. What (Simple Definition)

In asynchronous code using `async/await`, `try-catch` is used to **handle errors gracefully**.

> When using `await`, if a Promise **rejects**, the control jumps directly to the **`catch` block** — just like a thrown error in synchronous code.

---

### 🔹 2. Why (Purpose / Importance)

- Helps in writing **clean and readable** error-handling code.
- Avoids the use of `.catch()` chained after every promise.
- Lets you handle multiple awaited operations inside a single error-handling block.

---

### 🔹 3. How (Code Examples)

#### ✅ Without `try-catch` (Unhandled rejection)

```js
async function getUser() {
  const data = await fetchFromDB(); // If this fails, app may crash!
  console.log(data);
}
✅ With try-catch (Safe and handled)
js
Copy
Edit
async function getUser() {
  try {
    const data = await fetchFromDB();
    console.log(data);
  } catch (err) {
    console.error('Failed to get user:', err.message);
  }
}
🔹 4. Real-World Analogy
Using try-catch is like putting on a helmet while riding a bike —
you might still fall (error), but you’re protected (won’t crash the whole app).

🔹 5. Interview-Ready Summary
Concept	Description
try-catch	Used to catch errors in both sync and async code
await	Waits for a Promise to resolve or reject
Error handling	If the awaited promise fails, control jumps to catch
Good practice	Always use try-catch in async functions

💡 Tip
Wrap each await in a try-catch, or group them together if logical.

Use throw inside the catch if you want to propagate the error.

⚠️ Trap
You can’t catch errors from promises outside of async/await with try-catch:

js
Copy
Edit
// ❌ This will NOT be caught
try {
  fetch('https://bad-url').then(res => res.json());
} catch (err) {
  console.error('Will not catch here');
}
✅ Instead, use:

js
Copy
Edit
try {
  const res = await fetch('https://bad-url');
  const data = await res.json();
} catch (err) {
  console.error('Caught:', err.message);
}    