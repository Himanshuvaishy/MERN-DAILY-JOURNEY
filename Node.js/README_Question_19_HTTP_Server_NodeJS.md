# ✅ 19. How do you create a simple HTTP server in Node.js?

---

### 🔹 1. What (Simple Definition)

In Node.js, you can create a **basic web server** using the built-in **`http` module**, without needing any external framework.

> The `http` module allows your application to **listen to client requests** (like from a browser) and **respond** with data such as HTML, JSON, or plain text.

---

### 🔹 2. Why (Purpose / Importance)

- It’s the **foundation of any backend application** in Node.js.
- Helps you understand how the **request-response cycle** works.
- You can later build on this using frameworks like **Express.js**.

---

### 🔹 3. How (Step-by-Step with Example)

#### ✅ Step 1: Import the `http` module

```js
const http = require('http');

✅ Step 2: Create the server
js
Copy
Edit
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, Node.js Server!');
});
✅ Step 3: Start listening on a port
js
Copy
Edit
const PORT = 3000;

server.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
🔹 4. Full Working Code
js
Copy
Edit
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Welcome to my Node.js server!');
});

server.listen(3000, () => {
  console.log('Server listening at http://localhost:3000');
});
🔹 5. Real-World Analogy
Creating an HTTP server in Node.js is like building a reception desk:

It waits for someone to ring the bell (client request),

Then gives a response like "Welcome!" or a document (response).

🔹 6. Interview-Ready Summary
Concept	Description
Module used	http (built-in)
Method to create	http.createServer(callback)
Send response	Use res.writeHead() and res.end()
Start server	Use server.listen(port, callback)

💡 Tip
You can respond with HTML, JSON, or static files manually, but it’s easier with Express.js as your project grows.

⚠️ Trap
If you don’t call res.end(), the server will keep waiting and never send a response.

Always set the correct Content-Type header for the type of response you’re sending.