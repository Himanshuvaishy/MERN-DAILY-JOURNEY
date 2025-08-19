# Difference Between CommonJS (CJS) and ES Modules (MJS)

JavaScript has evolved with **two primary module systems**:  
- **CommonJS (CJS)** → Used traditionally in Node.js (`require`, `module.exports`).  
- **ES Modules (ESM/MJS)** → The modern standard (`import`, `export`).  

---

## 1. CommonJS (CJS) Modules

- **Synchronous file loading** → Loads one module at a time (good for server-side, not browsers).  
- **Imports are not hoisted** → Must define imports at the top, but they are executed at runtime.  
- **Top-level await is not allowed** → Async code must be wrapped in functions.  
- **Export syntax** → Multiple values can be exported using `module.exports = {}`.  
- **Strict mode** → Not enabled by default.  
- **File extension** → Optional (`.js` works; `.cjs` convention for clarity).  
- **Can load any file** if given full path (via `require`).  
- **package.json config** → `"type": "commonjs"` is optional (Node.js defaults to CJS).  
- **`this`** → Refers to `module.exports` by default.  

**Example:**
```js
// math.cjs
module.exports.add = (a, b) => a + b;

// app.cjs
const math = require('./math.cjs');
console.log(math.add(2, 3));

2. ES6 (ESM / MJS) Modules

Asynchronous file loading → Better for browsers and large apps.

Imports are hoisted → Resolved before code execution.

Top-level await is allowed → Can use await outside functions.

Export syntax → Multiple values can be exported using export.

Strict mode → Enabled by default.

File extension → Mandatory (.mjs, or .js if "type": "module" is set in package.json).

Can only load .js and .mjs files.

package.json config → Must set "type": "module" for .js files.

this → Is undefined in ESM scope.

Example:

// math.mjs
export function add(a, b) {
  return a + b;
}

// app.mjs
import { add } from './math.mjs';
console.log(add(2, 3));

## 3. Quick Comparison Table

| Feature               | CommonJS (CJS)                     | ES Modules (MJS/ESM)                 |
|------------------------|------------------------------------|---------------------------------------|
| Loading               | Synchronous                        | Asynchronous                          |
| Import behavior        | Not hoisted                        | Hoisted                               |
| Top-level await        | ❌ Not allowed                     | ✅ Allowed                            |
| Export syntax          | `module.exports`                   | `export` / `export default`           |
| Strict mode            | ❌ Not enabled by default          | ✅ Enabled by default                 |
| File extension         | Optional (`.cjs` convention)       | Mandatory (`.mjs` or `.js` with config) |
| File types supported   | Any (via `require`)                | Only `.js` / `.mjs`                   |
| Package.json config    | `"type": "commonjs"` optional      | `"type": "module"` required for `.js` |
| `this` in module scope | Points to `module.exports`         | `undefined`                           |

4. Summary

CJS (CommonJS): Default in Node.js, synchronous, older system.

MJS/ESM (ES6 Modules): Modern, async, standardized across browsers and Node.js.

Today, ESM is preferred for new projects, but CJS remains important for legacy support.