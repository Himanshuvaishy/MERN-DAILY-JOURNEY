# ✅ 22. What is routing in Express.js?

---

### 🔹 1. What (Simple Definition)

**Routing** in Express.js refers to **defining endpoints** in your web application — specifying how the app responds to different **HTTP requests** (like GET, POST, PUT, DELETE) on specific **URLs**.

> It’s like telling your server:  
> _"When someone visits this path with this method, here's what to do."_

---

### 🔹 2. Why (Purpose / Importance)

- Routes are **core to any backend system**.
- Let you define what happens when users visit different **URLs**.
- Help structure your app logically and clearly.

---

### 🔹 3. How (Example of Each Type)

#### ✅ Basic GET route

```js
app.get('/', (req, res) => {
  res.send('Welcome to the Home Page');
});

✅ POST route
js
Copy
Edit
app.post('/submit', (req, res) => {
  res.send('Form submitted');
});
✅ Route with URL Parameters
js
Copy
Edit
app.get('/user/:id', (req, res) => {
  res.send(`User ID is ${req.params.id}`);
});
✅ Route with Query Parameters
js
Copy
Edit
app.get('/search', (req, res) => {
  res.send(`Search term: ${req.query.q}`);
});
✅ Multiple Methods on One Route
js
Copy
Edit
app.route('/book')
  .get((req, res) => res.send('Get a book'))
  .post((req, res) => res.send('Add a book'))
  .put((req, res) => res.send('Update the book'));
  
🔹 4. Real-World Analogy
Routing is like a switchboard in a call center:
Depending on the number you press (GET /, POST /login), your request is routed to the appropriate handler (person).

🔹 5. Interview-Ready Summary
HTTP Method	Purpose
GET	Read data / fetch pages
POST	Submit or create new data
PUT	Update existing data
DELETE	Delete data

Routes map a URL pattern + HTTP method to a specific handler function.

Express makes routing declarative and readable.

💡 Tip
Use req.params for dynamic parts of the URL (e.g., /user/:id)

Use req.query to access URL query strings (e.g., /search?q=node)

⚠️ Trap
Routes are case-sensitive and order matters.

Always define more specific routes first, then general ones like *.