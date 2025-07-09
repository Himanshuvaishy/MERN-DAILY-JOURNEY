
# ✅ 8. What are Streams in Node.js?

---

### 🔹 1. What (Simple Definition)

A **stream** in Node.js is a powerful **interface for working with data piece by piece**, instead of loading it all at once.

> It allows you to **read**, **write**, or **transform** large amounts of data **efficiently and asynchronously**.

---

### 🔹 2. Why (Purpose / Importance)

- In real-world applications, you often deal with **large files**, **live video/audio**, or **network data**.
- **Loading it all at once can consume lots of memory and slow performance.**
- Streams let you **process data in chunks**, improving **performance**, **memory usage**, and **speed**.

---

### 🔹 3. How (Detailed Explanation)

Node.js provides **4 types of streams**:

| Type            | Description                         |
|------------------|-------------------------------------|
| `Readable`       | Stream from which data can be read  |
| `Writable`       | Stream to which data can be written |
| `Duplex`         | Can be both read from and written to (e.g., TCP sockets) |
| `Transform`      | A duplex stream that can modify or transform the data (e.g., compression) |

---

#### ✅ Example 1: Reading a File with Streams

```js
const fs = require('fs');

const readableStream = fs.createReadStream('bigfile.txt', 'utf8');

readableStream.on('data', (chunk) => {
  console.log('Received chunk:', chunk);
});

readableStream.on('end', () => {
  console.log('Finished reading file.');
});
```

🟢 This will **read the file in chunks** — not the whole file at once!

---

#### ✅ Example 2: Piping Readable to Writable

```js
const fs = require('fs');

const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');

readStream.pipe(writeStream);
```

🔁 This will **automatically stream data from input to output**, chunk by chunk.

---

### 🔹 4. Real-World Analogy

Imagine watching a **YouTube video**:
- You don’t wait for the **whole video to download**.
- Instead, it **streams small parts**, and plays while buffering more.

That’s how Node.js streams work — process data **on the go** instead of waiting for all of it.

---

### 🔹 5. Interview-Ready Summary

- **Streams** are used in Node.js to **handle large data efficiently** in chunks.
- Node has 4 stream types: **Readable, Writable, Duplex, and Transform**.
- You can use **events (`data`, `end`)** or **pipe()** for streaming operations.
- They help with **memory efficiency** and are heavily used in **file handling**, **HTTP servers**, and **networking**.

---

### 💡 Tip

Use `pipe()` when possible — it handles **backpressure** (when writable can't keep up with readable) automatically!

---

### ⚠️ Trap

Don’t confuse streams with buffers —  
- **Buffer** stores data temporarily,  
- **Stream** processes it continuously.
