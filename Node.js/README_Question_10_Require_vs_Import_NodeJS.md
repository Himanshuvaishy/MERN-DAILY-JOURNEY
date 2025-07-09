# ✅ 10. What is the Difference Between `require()` and `import` in Node.js?

---

### 🔹 1. What (Simple Definition)

Both `require()` and `import` are used to **include external modules** in Node.js, but:

- `require()` is part of **CommonJS**, the default module system in Node.js.
- `import` is part of **ES Modules (ESM)**, which is the standard in modern JavaScript (ES6+).

---

### 🔹 2. Why (Purpose / Importance)

Understanding the difference helps you:

- Use the **right syntax** for your project.
- Work with **modern JavaScript tools**.
- Ensure compatibility between different **module systems**.

---

### 🔹 3. How (Detailed Comparison)

| Feature            | `require()`                 | `import`                              |
| ------------------ | --------------------------- | ------------------------------------- |
| Module System      | CommonJS                    | ES Modules (ESM)                      |
| Syntax             | `const fs = require('fs')`  | `import fs from 'fs'`                 |
| File Extension     | Works with `.js`            | Requires `.mjs` or `"type": "module"` |
| Execution          | Synchronously loads modules | Asynchronously loads modules          |
| Top-level usage    | Can be used anywhere        | Must be at the top of the file        |
| Support in Node.js | Native (default)            | Requires setup (Node.js >= v12+)      |
| Dynamic Import     | Not supported               | Supported via `import()`              |

---

### ✅ Example Using `require()`

```js
// math.js
function add(a, b) {
  return a + b;
}
module.exports = add;

// app.js
const add = require('./math');
console.log(add(2, 3));  // Output: 5


  // math.js
export function add(a, b) {
  return a + b;
}

// app.mjs OR with "type": "module" in package.json
import { add } from './math.js';
console.log(add(2, 3));  // Output: 5

 🔹 4. Real-World Analogy
require() is like dialing a landline — always reliable and widely supported.

import is like using a smartphone app — modern, flexible, and better for larger apps.

🔹 5. Interview-Ready Summary
Use require() for CommonJS (default in Node.js).

Use import for ES Modules, especially in modern or frontend-like projects.

Both load external code but differ in syntax, behavior, and compatibility.

💡 Tip
If you’re using import, add "type": "module" to your package.json or rename files to .mjs.

⚠️ Trap
You can’t mix require() and import in the same file unless you're using dynamic import via import().
```
