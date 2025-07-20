# 33. How Does Node.js Handle Concurrent Requests?

---

## 🔍 What

Node.js handles **concurrent requests** using a **non-blocking, event-driven architecture** powered by the **event loop**.

This means:
- It can process **many requests at the same time** without creating a new thread for each one.
- Instead of blocking, it **registers callbacks** and continues with other work.

---

## 💡 Why

Traditional backend servers (like Apache, Java, PHP) create a new **thread** or **process** for each request, which:

- Uses more **memory and CPU**
- Doesn't scale well with thousands of users

Node.js avoids this by using **a single-threaded event loop** with **asynchronous callbacks**.

---

## ⚙️ How Node.js Handles It

1. A request comes in (e.g., via HTTP)
2. Node.js adds it to the **event queue**
3. If it's a **non-blocking operation** (e.g., DB read, file read):
   - The request is sent to the **libuv thread pool**
   - The main thread continues processing other requests
4. When the operation is complete, the **callback** is added to the event loop
5. Response is sent when the callback runs

---

### 🧪 Example

```js
const http = require('http');
const fs = require('fs');

http.createServer((req, res) => {
  fs.readFile('file.txt', (err, data) => {
    if (err) return res.end('Error!');
    res.end(data);
  });
}).listen(3000);
Even if 1000 users hit this at once, the event loop doesn’t block.

The file read runs in the background (libuv thread pool).

🧠 Key Concepts Involved
Concept	Role
Event loop	Core engine managing all async operations
Callback queue	Where completed tasks are queued for execution
Libuv	C library that Node.js uses for thread pool & I/O operations
Non-blocking I/O	I/O operations don't block the thread; continue when done

🔄 Example with Heavy Computation (BAD way)
js
Copy
Edit
// Blocking operation - NOT ideal for concurrent requests
http.createServer((req, res) => {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) sum += i;
  res.end('Done');
}).listen(3000);
❌ This blocks the event loop, and no other request can be served until it's done.

✅ Solution for Heavy Tasks
Offload to:

child_process or

worker_threads

📦 Summary
Node.js uses non-blocking I/O and a single-threaded event loop

Handles thousands of concurrent requests efficiently

Avoids blocking the main thread with asynchronous code

Use workers or child processes for heavy synchronous tasks

