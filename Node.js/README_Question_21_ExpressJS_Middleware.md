# ✅ 21. What is middleware in Express.js?

---

### 🔹 1. What (Simple Definition)

**Middleware** in Express.js refers to **functions that execute during the request-response cycle**.

> Middleware functions have access to the `req`, `res`, and `next` objects, and they can **modify the request or response**, **run code**, or **terminate the cycle**.

---

### 🔹 2. Why (Purpose / Importance)

Middleware makes Express:
- **Modular**: You can separate logic into reusable functions.
- **Powerful**: You can add **logging**, **authentication**, **validation**, **parsing**, and **error handling** cleanly.
- **Flexible**: You can stack multiple middlewares to create a pipeline.

---

### 🔹 3. How (Syntax & Examples)

#### ✅ Basic Middleware Example

```js
const express = require('express');
const app = express();

const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next(); // Pass to next middleware or route handler
};

app.use(logger);

app.get('/', (req, res) => {
  res.send('Home Page');
});

app.listen(3000);
 
 ✅ Built-in Middleware
express.json() – parses incoming JSON

express.urlencoded() – parses form data

express.static() – serves static files

js
Copy
Edit
app.use(express.json());
app.use(express.static('public'));
✅ Error Handling Middleware
js
Copy
Edit
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
🔹 4. Real-World Analogy
Middleware is like security checks and form verifications at an airport.
Each middleware checks, modifies, or validates something before passing the person (request) to the final destination (response).

🔹 5. Interview-Ready Summary
Type	Description
Application-level	Applied globally using app.use()
Router-level	Applied to specific routes
Built-in	Provided by Express like express.json()
Error-handling	Special middleware with 4 params
Third-party	Like morgan, cors, helmet, body-parser

💡 Tip
Always call next() unless you want to end the request.

Order of middleware matters — it's executed top to bottom.

⚠️ Trap
Forgetting to call next() can cause the app to hang.

Always define error middleware after all routes.