# Node.js Module System, CommonJS, and `module.exports` vs `exports`

## 1. What is the Module System in Node.js?

In Node.js, **modules** are separate files or pieces of code that can be reused in different parts of your application.  
They help keep code organized and avoid putting everything in one giant file.

### Types of Modules in Node.js:
1. **Core (Built-in) Modules** → Already included in Node.js (e.g., `fs`, `path`, `http`).
2. **Local Modules** → Files you create (e.g., `math.js`, `utils.js`).
3. **Third-Party Modules** → Installed via **npm** (e.g., `express`, `mongoose`).

---

## 2. CommonJS — The Default Module System in Node.js

Before ES6 introduced `import`/`export`, Node.js used **CommonJS**.

- **`require()`** → Loads modules.
- **`module.exports`** → Exports from a file.

**Example:**

`math.js`
```js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = { add, subtract };
```

`app.js`
```js
const math = require('./math');

console.log(math.add(5, 3)); // 8
console.log(math.subtract(5, 3)); // 2
```

---

## 3. `module.exports` vs `exports`

- **`module.exports`** → The actual object returned when you `require()` a file.
- **`exports`** → A shortcut (alias) to `module.exports`.

When Node.js starts a module, it internally does:
```js
let exports = module.exports = {};
```
So both `exports` and `module.exports` initially point to the same object.

---

### ✅ Example — Works Fine
`greet.js`
```js
exports.sayHello = function(name) {
    return `Hello, ${name}!`;
};
```
`app.js`
```js
const greet = require('./greet');
console.log(greet.sayHello("Himanshu")); // Hello, Himanshu!
```

---

### ❌ Example — Common Mistake
`bad.js`
```js
exports = function() {
    console.log("This won't work as expected!");
};
```
`app.js`
```js
const bad = require('./bad');
console.log(bad); // {}
```
**Why?** Because reassigning `exports` breaks its link to `module.exports`.

---

## 4. When to Use Which?

- **`module.exports`** → Export one thing (function, class, object, etc.)
- **`exports`** → Export multiple named things.

---

### Example — Single Export
`logger.js`
```js
module.exports = function(message) {
    console.log(`[LOG]: ${message}`);
};
```
`app.js`
```js
const logger = require('./logger');
logger("Server started");
```

---

### Example — Multiple Exports
`math.js`
```js
exports.add = (a, b) => a + b;
exports.subtract = (a, b) => a - b;
```
`app.js`
```js
const math = require('./math');
console.log(math.add(4, 2));  // 6
```

---

## 5. Quick Summary Table

| Feature              | module.exports                | exports                         |
|----------------------|--------------------------------|----------------------------------|
| Default in Node.js   | ✅ Yes                         | ❌ No (just an alias)            |
| Export single item   | ✅ Perfect choice              | ❌ Not recommended               |
| Export multiple items| ✅ Works                       | ✅ Works                         |
| Can reassign         | ✅ Yes                         | ❌ No (breaks the link)          |

---

## 6. Hidden Magic — How Node.js Wraps Modules

Every file in Node.js is wrapped like this before execution:
```js
(function (exports, require, module, __filename, __dirname) {
    // Your file content here
});
```
That’s why `exports`, `require`, `module`, `__filename`, and `__dirname`  
are available without you importing them.

- **`module`** → Represents the current file.
- **`exports`** → Shortcut to `module.exports`.
- **`require`** → Function to import modules.
- **`__filename`** → Absolute path of the current file.
- **`__dirname`** → Absolute path of the current directory.
