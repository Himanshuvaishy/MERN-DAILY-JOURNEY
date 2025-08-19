# 🔹 Types of Modules in Node.js

---

## 1. Core (Built-in) Modules

These are provided by **Node.js itself**.  
You don’t need to install them; just `require` or `import` directly.

**Examples:** `fs`, `http`, `path`, `crypto`, `os`, etc.

```js
// Example: fs (file system)
const fs = require('fs');

// Writing to a file
fs.writeFileSync('demo.txt', 'Hello, Himanshu!');

// Reading from a file
const data = fs.readFileSync('demo.txt', 'utf8');
console.log(data);
✅ Use them when you want system-level functionality (files, OS info, network, etc.).

2. Local (User-defined) Modules
These are the modules you create yourself in separate files.
Helps break your project into smaller, reusable parts.

math.js

js
Copy
Edit
function add(a, b) {
  return a + b;
}

module.exports = { add };
app.js

js
Copy
Edit
const math = require('./math');
console.log(math.add(2, 3)); // 5
✅ Use them to organize your own project code.

3. Third-Party (External) Modules
Installed using npm (Node Package Manager).
They extend Node.js with powerful libraries built by the community.

Example: Express.js

bash
Copy
Edit
npm install express
js
Copy
Edit
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Express!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
✅ Use them when you don’t want to reinvent the wheel (web servers, databases, UI tools, etc.).

