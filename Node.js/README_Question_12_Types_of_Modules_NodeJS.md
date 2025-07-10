
# ✅ 12. What are the Types of Modules in Node.js?

---

### 🔹 1. What (Simple Definition)

In Node.js, **modules** are reusable blocks of code that help break down applications into smaller, manageable pieces.  
Node.js supports **three types of modules**:

1. **Core Modules**
2. **Local Modules**
3. **Third-Party Modules**

---

### 🔹 2. Why (Purpose / Importance)

Understanding the types of modules is crucial because:
- You’ll know **where the module comes from** (built-in, created, or installed).
- It helps structure your app better.
- You can **reuse and share** code easily.

---

### 🔹 3. How (Types Explained)

#### ✅ 1. **Core Modules**
- Built into Node.js
- No need to install separately
- Examples: `fs`, `http`, `path`, `os`, `events`

```js
const fs = require('fs');
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

---

#### ✅ 2. **Local Modules**
- Files or folders you create in your own project
- Used to encapsulate app-specific logic
- Imported with relative path `./` or `../`

```js
// math.js
function multiply(a, b) {
  return a * b;
}
module.exports = multiply;

// app.js
const multiply = require('./math');
console.log(multiply(2, 5));
```

---

#### ✅ 3. **Third-Party Modules**
- Installed via **npm (Node Package Manager)**
- Helps avoid reinventing the wheel
- Examples: `express`, `mongoose`, `dotenv`, `lodash`

```bash
npm install express
```

```js
const express = require('express');
const app = express();
```

---

### 🔹 4. Real-World Analogy

Imagine building a computer:
- **Core modules** = built-in components (like CPU, motherboard)
- **Local modules** = custom modifications you build
- **Third-party modules** = components you buy and install (like a graphics card or software)

---

### 🔹 5. Interview-Ready Summary

- Node.js modules are classified into:
  - **Core modules** (built-in),
  - **Local modules** (you create),
  - **Third-party modules** (installed via npm).
- This system improves **modularity**, **reuse**, and **scalability**.

---

### 💡 Tip

Use `npm list` to see installed third-party modules.  
Use `require('module-name')` to include any module.

---

### ⚠️ Trap

Don’t forget:
- You **must use `./`** for local files — omitting it makes Node search in core/third-party.
