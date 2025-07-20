# 29. What are Child Processes in Node.js?

---

## 🔍 What

A **child process** in Node.js is a way to **run a separate script or system command** outside the main Node.js process.  
This allows you to perform tasks like:

- Executing shell commands
- Running CPU-intensive tasks in parallel
- Launching other Node.js scripts or system binaries

Node.js provides the **`child_process` module** to spawn child processes.

---

## 💡 Why

Node.js is **single-threaded**, so it’s not ideal for **CPU-heavy tasks** (e.g., compression, encryption, large file parsing).  
Using child processes helps you:

- Run those tasks in **parallel**, without blocking the main thread
- **Offload heavy operations** and keep your app responsive
- Interface with system-level tools (like Git, Python, ffmpeg, etc.)

---

## ⚙️ How

Node.js provides 4 key methods from the `child_process` module:

| Method        | Use Case                                     |
|---------------|----------------------------------------------|
| `spawn()`     | For long-running processes with streams      |
| `exec()`      | For simple commands (buffered output)        |
| `execFile()`  | Similar to exec but directly runs files      |
| `fork()`      | Specifically for spawning Node.js scripts    |

---

### ✅ Example: Using `exec()` to run a shell command

```js
const { exec } = require('child_process');

exec('node -v', (err, stdout, stderr) => {
  if (err) {
    console.error(`Error: ${err.message}`);
    return;
  }
  if (stderr) {
    console.error(`stderr: ${stderr}`);
    return;
  }
  console.log(`Node version: ${stdout}`);
});
✅ Example: Using fork() to run a separate Node.js script
main.js

js
Copy
Edit
const { fork } = require('child_process');

const child = fork('child.js');

child.on('message', (msg) => {
  console.log('Message from child:', msg);
});

child.send('Hey child process!');
child.js

js
Copy
Edit
process.on('message', (msg) => {
  console.log('Message from parent:', msg);
  process.send('Hello from child!');
});
🔐 Use Cases
Running batch jobs or scripts

Heavy computation (e.g., image processing, video encoding)

Managing multiple worker processes

Running Python/PHP scripts inside Node.js apps

🚨 Caution
Use child processes only when necessary (they are memory-intensive)

Avoid excessive spawning of processes — use pools or worker threads if needed

📦 Summary
child_process lets you run other programs from Node.js

Great for running heavy or isolated tasks

Use spawn, exec, execFile, or fork based on your use case

