# Controlling Data Transfer Speed & Backpressure in Node.js

## ⚙️ Introduction
When you pipe data from one stream to another (like from a file to a socket), Node.js transfers data at **maximum possible speed** and automatically handles **backpressure**.

However, if you want to **manually control or slow down** the speed of data transfer, you need to handle backpressure yourself using `pause()` and `resume()` methods on the readable stream.

---

## 🧠 What is Backpressure?
**Backpressure** occurs when the **destination** (like a socket or writable stream) cannot process incoming data as quickly as the **source** (like a file read stream) sends it.

In simple words — data flows too fast for the receiver to handle, causing buffer overflow.

### Node.js handles this automatically in `.pipe()`:
```js
readStream.pipe(socket);
```
✅ Node pauses and resumes reading automatically based on the writable stream’s (`socket`) buffer status.

---

## 🐢 Manual Control (Throttle the Speed)

If you want to slow down or simulate a slower connection, you can manually pause the stream for a few milliseconds before resuming.

### Example — Slow File Transfer
```js
const fs = require('fs');
const net = require('net');

const server = net.createServer(async (socket) => {
  const readStream = fs.createReadStream('largefile.txt');

  readStream.on('data', (chunk) => {
    const canSend = socket.write(chunk);

    // Pause for 10ms to slow down transfer
    readStream.pause();
    setTimeout(() => {
      readStream.resume();
    }, 10);

    // If socket buffer is full, wait for 'drain'
    if (!canSend) {
      socket.once('drain', () => readStream.resume());
    }
  });

  readStream.on('end', () => {
    console.log('File fully sent');
    socket.end();
  });
});

server.listen(8080, () => console.log('Server running on port 8080'));
```

### 🔍 Explanation
1. `readStream` reads chunks from the file.
2. Each chunk is sent to the socket using `socket.write()`.
3. After writing a chunk, we **pause** reading for 10 milliseconds.
4. Then we **resume** reading, slowing the overall transfer rate.

---

## ⚡ Automatic Backpressure Handling with `.pipe()`
If you just use:
```js
readStream.pipe(socket);
```
Node handles backpressure automatically — pausing when the socket’s internal buffer is full, and resuming when the `'drain'` event fires.

This is the **recommended** way for most real applications.

---

## 🌐 Browser Backpressure Behavior
Browsers also handle backpressure automatically when downloading or streaming data.

- When the browser’s buffer is full, it **pauses** reading from the network.
- Once it clears enough space, it **resumes** the stream.
- In HTTP/2 and newer protocols, flow control is managed using **window update frames**, letting the client tell the server how much data it can handle.

🧠 **So even if the server sends data quickly, the browser might throttle internally** to maintain stability and performance.

---

## ✅ Summary

| Concept | Description |
|----------|--------------|
| **Backpressure** | Happens when data is sent faster than it can be processed. |
| **Automatic Handling** | `pipe()` handles it automatically in Node.js. |
| **Manual Control** | Use `pause()` and `resume()` with `setTimeout()` for throttling. |
| **Use Case** | Simulate slow connections, limit bandwidth, test performance. |
| **Browser Behavior** | Browsers automatically pause/resume to manage data flow. |

---

## 🧩 Example Recap

```js
readStream.on("data", (chunk) => {
    socket.write(chunk);
    readStream.pause();
    setTimeout(() => {
        readStream.resume();
    }, 10);
});
```

**Explanation:**
- The stream sends a chunk.
- Pauses for 10ms.
- Resumes again → thus controlling speed.

---

## ⚠️ Key Notes
- `socket.write()` returns `false` when the internal buffer is full. Use `'drain'` to know when to resume.
- Always handle `'error'` and `'end'` events to prevent memory leaks.
- For production-level throttling, consider using libraries like `stream-throttle`.

---

**Created by:** GPT-5  
**Topic:** Controlling Data Transfer Speed & Backpressure in Node.js Streams  
**Date:** 2025
