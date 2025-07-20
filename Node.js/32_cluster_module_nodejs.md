# 32. What is the Purpose of the `cluster` Module in Node.js?

---

## 🔍 What

The **`cluster` module** in Node.js allows you to **create multiple processes** that **share the same server port**.

These processes are called **workers**, and each one runs in its **own thread** — giving your application the ability to use **all CPU cores**.

---

## 💡 Why

Node.js is single-threaded and by default, it **uses only one CPU core**.  
So on multi-core machines, you’re **wasting CPU power** unless you use `cluster`.

> ✅ With `cluster`, you can scale your app horizontally — one worker per CPU core.

---

## ⚙️ How It Works

- Master process manages **worker processes**
- Workers are created using `cluster.fork()`
- Each worker can handle requests independently
- Ideal for **web servers** and **high-concurrency apps**

---

## 🧪 Example: Basic Cluster Usage

```js
// server.js
const cluster = require('cluster');
const http = require('http');
const os = require('os');

const numCPUs = os.cpus().length;

if (cluster.isMaster) {
  console.log(`Master process ${process.pid} is running`);

  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  // Listen for dying workers
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
  });
} else {
  // Worker processes have their own event loop
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end(`Handled by worker: ${process.pid}`);
  }).listen(3000);

  console.log(`Worker ${process.pid} started`);
}
✅ Output
If you have 4 CPU cores, this will create 4 workers.
Each one will handle HTTP requests on port 3000.

🧠 Key Methods
Method	Description
cluster.isMaster	Checks if current process is master
cluster.fork()	Creates a new worker process
cluster.on('exit')	Listen when a worker dies

🚀 Benefits
Utilizes multiple CPU cores

Handles more concurrent users

Improves performance and reliability

⚠️ Limitations
No shared memory between workers

You must handle session sharing, load balancing, etc.

Not ideal for apps with a lot of state in memory

📦 Summary
cluster lets Node.js scale across CPU cores

Each worker handles its own set of requests

Best used for high-load servers