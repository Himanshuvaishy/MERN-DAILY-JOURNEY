# ✅ 25. What is a callback function in Node.js?

---

### 🔹 1. What (Simple Definition)

A **callback function** is a **function passed as an argument** to another function.  
It gets **executed after some operation is completed**, usually asynchronously.

> In Node.js, callbacks are commonly used for **handling asynchronous tasks** like reading files, querying databases, or making API calls.

---

### 🔹 2. Why (Purpose / Importance)

- JavaScript (and Node.js) is **non-blocking** and **event-driven**.
- Callbacks allow you to:
  - Handle tasks **after** something finishes (e.g., file read)
  - Keep code **non-blocking and efficient**
  - Control **execution order** in async flows

---

### 🔹 3. How (Basic Example)

#### ✅ Simple callback example

```js
function greetUser(name, callback) {
  console.log('Hello', name);
  callback();
}

function sayGoodbye() {
  console.log('Goodbye!');
}

greetUser('Himanshu', sayGoodbye);

// Output:
// Hello Himanshu
// Goodbye!

✅ Async callback (with fs.readFile)
js
Copy
Edit
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log('File content:', data);
});
🔹 4. Real-World Analogy
A callback is like telling your friend:
"Hey, after you buy the tickets, call me."
Your friend (function) does something, and then calls you back when done.

🔹 5. Interview-Ready Summary
Concept	Description
Callback	Function passed as an argument to be executed later
Use case	Used in async operations like reading files or APIs
Pattern	function(param, callback) { ... callback(result) }

💡 Tip
Use arrow functions or named functions as callbacks.

Node.js convention is:
callback(error, result) — error-first.

⚠️ Trap
Using many nested callbacks = "callback hell" 😱

Can be avoided using Promises or async/await.