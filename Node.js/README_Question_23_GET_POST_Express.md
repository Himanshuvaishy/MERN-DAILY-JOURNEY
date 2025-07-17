# ✅ 23. How do you handle GET and POST requests in Express?

---

### 🔹 1. What (Simple Definition)

In Express.js, you handle **GET** and **POST** requests using:

- `app.get()` → for reading or retrieving data  
- `app.post()` → for submitting or creating data  

> These are the **most commonly used HTTP methods** in web apps.

---

### 🔹 2. Why (Purpose / Importance)

- **GET** is used when fetching data from the server (e.g., show a page or user info).
- **POST** is used when **sending data to the server** (e.g., form submissions, creating resources).

They form the backbone of the **client-server communication model**.

---

### 🔹 3. How (Code Examples)

#### ✅ Handle a GET request

```js
app.get('/', (req, res) => {
  res.send('GET request received!');
});

✅ Handle a POST request (with JSON body)
js
Copy
Edit
app.use(express.json()); // Middleware to parse JSON

app.post('/submit', (req, res) => {
  const userData = req.body;
  res.send(`POST received: ${JSON.stringify(userData)}`);
});
✅ Handle POST request (with form data)
js
Copy
Edit
app.use(express.urlencoded({ extended: true }));

app.post('/contact', (req, res) => {
  const { name, email } = req.body;
  res.send(`Form submitted by ${name} with email ${email}`);
});
🔹 4. Real-World Analogy
Think of GET like reading a book, and POST like writing in a notebook.
One retrieves data, the other sends data.

🔹 5. Interview-Ready Summary
Method	Description
GET	Retrieve data from server
POST	Send data to server (e.g., create)
req.body	Access data sent in POST request
express.json()	Middleware to parse JSON
express.urlencoded()	Parse form data

💡 Tip
Always use express.json() and/or express.urlencoded() for parsing req.body.

Use Postman or curl to test POST requests manually.

⚠️ Trap
If you forget to use body-parsing middleware, req.body will be undefined in POST requests.

Don’t expose sensitive data in query strings — use POST instead.