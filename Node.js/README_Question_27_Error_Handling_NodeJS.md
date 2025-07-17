# ✅ 27. How do you handle errors in Node.js?

---

### 🔹 1. What (Simple Definition)

**Error handling** in Node.js means **catching and responding to errors** that occur during execution — especially in **asynchronous code**.

> Proper error handling ensures your application doesn’t crash and helps you debug or inform the user gracefully.

---

### 🔹 2. Why (Purpose / Importance)

- Prevents application crashes
- Logs and reports issues for debugging
- Gives users meaningful feedback
- Keeps code clean, maintainable, and safe

---

### 🔹 3. How (Different Techniques)

#### ✅ 1. Using `try...catch` for synchronous or async/await code

```js
try {
  const result = riskyFunction();
  console.log(result);
} catch (err) {
  console.error('Error caught:', err.message);
}
✅ 2. Error handling in async/await
js
Copy
Edit
async function fetchData() {
  try {
    const data = await someAsyncTask();
    console.log(data);
  } catch (err) {
    console.error('Async error:', err.message);
  }
}
✅ 3. Using error-first callbacks (Node.js style)
js
Copy
Edit
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Failed to read file:', err.message);
    return;
  }
  console.log('File content:', data);
});
✅ 4. Global error handler in Express.js
js
Copy
Edit
app.use((err, req, res, next) => {
  console.error('Error middleware:', err.message);
  res.status(500).send('Something went wrong!');
});
🔹 4. Real-World Analogy
Think of error handling like seatbelts and airbags in a car.
They won’t stop a crash, but they prevent the crash from being fatal (crashing the app).

🔹 5. Interview-Ready Summary
Technique	Use Case
try...catch	Synchronous & async/await code
Error-first callbacks	Classic Node.js style (e.g., fs module)
process.on('uncaughtException')	Catch unhandled errors (last resort)
Express error middleware	Handle errors in routes or middleware

💡 Tip
Always return or exit after handling an error to avoid unexpected behavior.

Use centralized error logging (e.g., Winston, Sentry) in production.

⚠️ Trap
Don’t ignore the err object in callbacks.

Never throw errors in asynchronous callbacks — use return callback(err) instead.
