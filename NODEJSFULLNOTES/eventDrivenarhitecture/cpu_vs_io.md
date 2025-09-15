
# CPU Operations vs I/O Operations – Notes

## CPU Operations (Compute-Bound)
- Tasks that rely heavily on **processor (CPU)**.  
- **Examples**:  
  - Math calculations  
  - Sorting arrays  
  - Image/Video processing  
  - Encryption/Compression  
- **Characteristics**:  
  - Fast (in memory)  
  - Block event loop if heavy  
  - Node.js not ideal for CPU-heavy tasks → use **Worker Threads/Clustering**  

---

## I/O Operations (I/O-Bound)
- Tasks that involve **external resources** (disk, network, devices).  
- **Examples**:  
  - Reading/Writing files  
  - Database queries  
  - HTTP requests  
  - Logging  
- **Characteristics**:  
  - Slow (waiting on disk/network)  
  - Node.js handles them **asynchronously** (non-blocking)  
  - Uses **callbacks, promises, async/await**  

---

## Key Differences
| Feature               | CPU Operations                | I/O Operations              |
|------------------------|-------------------------------|-----------------------------|
| **Resource**           | CPU/Processor                 | Disk, Network, Devices      |
| **Nature**             | Compute-bound                 | I/O-bound                   |
| **Speed**              | Fast in memory                | Slower (waiting involved)   |
| **Blocking?**          | Can block event loop          | Non-blocking in Node.js     |
| **Handling in Node.js**| Worker Threads, Clustering    | Async I/O, Event Loop       |

---

## Code Examples

### CPU Operation (Blocking)
```js
function fibonacci(n) {
  if (n < 2) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(40)); // Heavy CPU task, blocks event loop
```

### I/O Operation (Non-Blocking)
```js
const fs = require('fs');

fs.readFile('demo.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log("File Content:", data);
});

console.log("Reading file..."); // Runs immediately
```

---

✅ **Summary:**  
- **CPU Ops** → Need more processing → may block event loop → use **Worker Threads**.  
- **I/O Ops** → External resource wait → Node.js excels (async, non-blocking).  
