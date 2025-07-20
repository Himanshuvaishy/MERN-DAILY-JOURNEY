# 30. How Does Node.js Handle Child Threads?

---

## 🔍 What

Node.js is **single-threaded** by design, using the **event loop** and **non-blocking I/O** to handle concurrency.  
But for **CPU-intensive** or **parallel processing**, it provides ways to create **child threads** through:

- ✅ `child_process` module (separate processes)
- ✅ `worker_threads` module (true threads since Node.js v10.5+)

---

## 💡 Why

Some tasks (like image processing, encryption, or data crunching) are **too heavy** for the main thread.

> Offloading such tasks to **child threads** prevents the **event loop** from being blocked.

---

## ⚙️ How Node.js Handles Child Threads

| Method           | Description                                              |
|------------------|----------------------------------------------------------|
| `child_process`  | Creates **separate processes** (more memory, isolated)   |
| `worker_threads` | Creates **actual threads** within the same process       |

---

### ✅ 1. `child_process` (separate process, own memory)

- Used for **isolation** and executing external scripts or binaries.
- Communicates via messaging (IPC).
- Good for: background jobs, scripting tools, running Python, etc.

> ✔️ We've already seen this in Q29 using `fork()` and `exec()`.

---

### ✅ 2. `worker_threads` (real threads within the same process)

Introduced in **Node.js v10.5.0+**  
Enables running **multiple threads** inside the same Node.js process.

### ➕ Benefits:
- Shared memory (faster communication than processes)
- Lower overhead than spawning new processes

---

### 🧪 Example: `worker_threads`

```js
// main.js
const { Worker } = require('worker_threads');

const worker = new Worker('./worker.js');

worker.on('message', (msg) => {
  console.log('From worker:', msg);
});

worker.postMessage('Start work');
js
Copy
Edit
// worker.js
const { parentPort } = require('worker_threads');

parentPort.on('message', (msg) => {
  // simulate heavy task
  let sum = 0;
  for (let i = 0; i < 1e8; i++) sum += i;
  parentPort.postMessage(`Sum: ${sum}`);
});
⚠️ child_process vs worker_threads
Feature	child_process	worker_threads
Isolation	Full separate process	Shared memory/thread
Communication	Slower (IPC)	Fast (same memory)
Use Case	Run CLI scripts, jobs	Compute-heavy tasks
Memory/CPU Overhead	Higher	Lower
Best for	Script exec, services	Parallel computation

🛡️ When to Use Threads in Node.js
When computation is too heavy for the main thread

When you want parallelism but within the same process

When you want to avoid spawning a full process

📦 Summary
Node.js is single-threaded but allows parallelism using:

child_process: creates separate processes

worker_threads: creates threads in the same process

Use threads to offload heavy tasks and keep the app responsive