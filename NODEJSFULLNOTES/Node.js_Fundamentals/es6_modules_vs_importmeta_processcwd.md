# ES6 Module System, `import.meta`, and `process.cwd()` in Node.js

## 1. ES6 Module System

The **ES6 module system** (also called **ECMAScript Modules - ESM**) is the modern way of handling modules in JavaScript, supported in browsers and Node.js.

### Key Features:
- Uses `import` and `export` keywords.
- Static imports (resolved at compile time).
- Strict mode by default.

### Example:
**math.js**
```js
export function add(a, b) {
  return a + b;
}

export const PI = 3.14;
```

**app.js**
```js
import { add, PI } from './math.js';

console.log(add(2, 3)); // 5
console.log(PI);        // 3.14
```

👉 Unlike CommonJS (`require`/`module.exports`), ES Modules support **tree-shaking** and are more optimized for modern JavaScript environments.

---

## 2. `import.meta`

`import.meta` is a special object in ES Modules that contains metadata about the current module.

### Example:
```js
// file: demo.js (must be an ES Module)
console.log(import.meta);
```

Output (example):
```js
{
  url: 'file:///Users/himanshu/projects/demo.js'
}
```

### Common Usage:
- `import.meta.url` → Absolute URL of the current module file.
- Helpful when you need the file path in ESM (since `__filename` and `__dirname` are **not available** in ESM).

**Example:**
```js
import { fileURLToPath } from 'url';
import { dirname } from 'path';

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

console.log(__filename); // full file path
console.log(__dirname);  // directory path
```

---

## 3. `process.cwd()`

In Node.js, `process.cwd()` returns the **current working directory** of the Node.js process.  
It tells you from where the script was executed, not the script's actual location.

### Example:
```js
// run with: node /Users/himanshu/projects/demo.js
console.log(process.cwd());
```

Output:
```
/Users/himanshu/projects
```

### Key Point:
- `process.cwd()` is based on where you **run Node**, not where the file lives.

---

## 4. Difference Between `import.meta.url` and `process.cwd()`

| Feature              | `import.meta.url`                        | `process.cwd()`                     |
|----------------------|------------------------------------------|--------------------------------------|
| **Belongs to**       | ES Modules (ESM)                        | Node.js process object               |
| **Returns**          | URL of the current module file           | Current working directory of process |
| **Scope**            | Specific to the module                   | Global to the process                |
| **Use case**         | Get module’s absolute file path          | Determine execution directory        |
| **Availability**     | Only in ESM                              | In both CommonJS and ESM             |

---

## 5. Summary

- **ES6 Modules** use `import`/`export` instead of `require`/`module.exports`.
- **`import.meta`** provides metadata (like the module’s absolute URL).  
- **`process.cwd()`** gives the current working directory where Node was started.  
- Use `import.meta.url` for **file path of the module**, and `process.cwd()` for **execution context path**.
