# 31. Difference Between `process.nextTick()`, `setImmediate()`, and `setTimeout()` in Node.js

---

## 🔍 What

These three functions are used to **schedule code execution** in the Node.js event loop — but they run at **different phases** and **timings**.

| Method             | When It Executes                                 |
|--------------------|--------------------------------------------------|
| `process.nextTick()` | Immediately **after the current operation**, before any I/O |
| `setImmediate()`     | **On the next iteration** of the event loop (after I/O) |
| `setTimeout()`       | After **at least X milliseconds**, in the **timers** phase |

---

## 💡 Why

Node.js is non-blocking and event-driven.  
Sometimes you need to **defer execution**, but choosing the **right function** affects timing and performance.

---

## 🧠 Event Loop Phases (Simplified)

1. **Timers** (→ `setTimeout`)
2. **Pending Callbacks**
3. **Idle/Prepare**
4. **Poll**
5. **Check** (→ `setImmediate`)
6. **Close Callbacks**
7. **microtasks** (→ `process.nextTick`, Promises)

---

## ⚙️ How They Work (Examples)

---

### 🔁 `process.nextTick()`

```js
console.log('Start');

process.nextTick(() => {
  console.log('process.nextTick');
});

console.log('End');

// Output:
// Start
// End
// process.nextTick
✅ Runs immediately after current code, before other queued tasks.

🔁 setImmediate()
js
Copy
Edit
console.log('Start');

setImmediate(() => {
  console.log('setImmediate');
});

console.log('End');

// Output:
// Start
// End
// setImmediate
✅ Runs on the next loop iteration, after I/O events.

🔁 setTimeout()
js
Copy
Edit
console.log('Start');

setTimeout(() => {
  console.log('setTimeout');
}, 0);

console.log('End');

// Output:
// Start
// End
// setTimeout
✅ Also runs after current stack, but after a minimum delay (often ~1ms even with 0).

🧪 Comparison Example
js
Copy
Edit
setTimeout(() => console.log('setTimeout'), 0);
setImmediate(() => console.log('setImmediate'));
process.nextTick(() => console.log('nextTick'));

// Output order:
// nextTick
// setTimeout OR setImmediate (depends on system + load)
🛑 Caution
Avoid putting recursive process.nextTick() calls — they can block the event loop (starvation).

Use setImmediate() when you want to schedule after I/O.

Use setTimeout(fn, 0) when you want a minimal delay but not immediate execution.

📦 Summary
Feature	process.nextTick()	setImmediate()	setTimeout()
Executes	Before next event loop	In "check" phase	In "timers" phase
Use Case	Urgent micro-tasks	Run after I/O	Scheduled delay
Risk	Starves event loop	Predictable timing	Not always precise