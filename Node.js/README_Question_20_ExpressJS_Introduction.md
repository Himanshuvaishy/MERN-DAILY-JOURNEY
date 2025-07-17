# ✅ 20. What is Express.js and why is it used?

---

### 🔹 1. What (Simple Definition)

**Express.js** is a **minimal and flexible web application framework** for Node.js.

> It provides a robust set of features to build **web applications** and **RESTful APIs**, simplifying the process of handling requests, responses, routes, and middleware.

---

### 🔹 2. Why (Purpose / Importance)

- **Node.js without Express** is powerful but low-level — you handle HTTP servers manually.
- **Express.js** abstracts and simplifies:
  - Routing
  - Middleware handling
  - Request/response management
  - Error handling
- It helps build scalable and maintainable server-side apps **quickly and efficiently**.

---

### 🔹 3. How (Getting Started Example)

#### ✅ Step 1: Install Express

```bash
npm install express

✅ Step 2: Create a basic server
js
Copy
Edit
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello from Express.js!');
});

app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}`);
});
🔹 4. Real-World Analogy
Think of Express.js as a framework on top of Node’s http module that gives you tools, shortcuts, and a clean structure — like jQuery did for vanilla JavaScript.

🔹 5. Interview-Ready Summary
Feature	What it Does
express()	Creates an Express application
app.get()	Handles GET requests
app.post()	Handles POST requests
app.listen()	Starts the server
Middleware support	Easily handle parsing, logging, auth, etc.
Routing system	Cleanly handle URLs and API paths

💡 Tip
Use middleware like express.json() or express.urlencoded() to parse incoming request data.

Combine Express with MongoDB, Mongoose, etc. to build full-stack apps.

⚠️ Trap
Don’t forget to use app.use(express.json()) when dealing with JSON requests.

Using too many middlewares in the wrong order can lead to bugs.