# ✅ 24. What is CORS and how do you enable it in Node.js?

---

### 🔹 1. What (Simple Definition)

**CORS** stands for **Cross-Origin Resource Sharing**.

> It’s a **security feature** implemented by browsers that restricts web pages from making **requests to a different domain** than the one that served the web page.

---

### 🔹 2. Why (Purpose / Importance)

- Prevents **unauthorized access** to resources from unknown or untrusted sources.
- Allows a server to specify who can access its data (which origin/domain).
- Without CORS enabled, **API requests from other domains will be blocked** by the browser.

---

### 🔹 3. Real-World Analogy

> Imagine you’re in a bank (the server) — only people with **valid ID cards (origins)** are allowed to interact with it.  
CORS is the **security guard** that checks the ID (Origin) before allowing the transaction.

---

### 🔹 4. How to Enable CORS in Node.js (with Express)

#### ✅ Step 1: Install the CORS package

```bash
npm install cors

✅ Step 2: Use it in your Express app
js
Copy
Edit
const express = require('express');
const cors = require('cors');

const app = express();

// Enable CORS for all routes
app.use(cors());

app.get('/', (req, res) => {
  res.send('CORS enabled!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
✅ Enable CORS for specific origins
js
Copy
Edit
const corsOptions = {
  origin: 'http://example.com'
};

app.use(cors(corsOptions));
🔹 5. Interview-Ready Summary
Concept	Explanation
CORS	Browser policy for cross-domain security
Problem	Blocks requests to different origins by default
Solution	Use cors middleware in Express
Allow all	app.use(cors())
Allow some	app.use(cors({ origin: 'http://example.com' }))

💡 Tip
For development, allow all origins using app.use(cors()).

In production, always restrict to specific trusted domains.

⚠️ Trap
CORS errors happen on the browser, not the server — the server might be working fine!

You must set CORS before defining your routes.