# 🔹 Transferring ArrayBuffer Data (Memory ➝ Disk & Network)

---

## 1. ArrayBuffer in Memory
- An **ArrayBuffer** is just a **block of memory** inside your program.  
- By itself, it’s **not useful** until you either:  
  1. Save it to a **file (disk)**, or  
  2. Send it over the **network (TCP, HTTP, WebSocket, etc.)**.  

👉 To transfer it, we usually wrap it in **TypedArray** or **Buffer**.

---

## 2. Writing ArrayBuffer to Disk (File System)

In Node.js, the `fs` module expects a **Buffer**, not ArrayBuffer.  
So we convert:

```js
const fs = require("fs");

// Step 1: Create ArrayBuffer
let ab = new ArrayBuffer(8);
let view = new Uint8Array(ab);
view[0] = 72; // ASCII 'H'
view[1] = 105; // ASCII 'i'

// Step 2: Convert ArrayBuffer → Buffer
let nodeBuffer = Buffer.from(ab);

// Step 3: Write to disk
fs.writeFileSync("hello.bin", nodeBuffer);
console.log("File written!");
```

✅ Now the ArrayBuffer is stored as a **binary file** on disk.

---

## 3. Reading Back from Disk
```js
const buf = fs.readFileSync("hello.bin");

// Convert Node.js Buffer → ArrayBuffer
let ab2 = buf.buffer.slice(buf.byteOffset, buf.byteOffset + buf.byteLength);
let view2 = new Uint8Array(ab2);

console.log(view2); // Uint8Array(2) [72, 105] → "Hi"
```

---

## 4. Sending ArrayBuffer Over Network

### TCP Example
```js
const net = require("net");

const server = net.createServer(socket => {
  socket.on("data", (data) => {
    console.log("Received:", new Uint8Array(data.buffer));
  });

  // Send ArrayBuffer to client
  let ab = new ArrayBuffer(4);
  let view = new Uint8Array(ab);
  view.set([1, 2, 3, 4]);
  socket.write(Buffer.from(ab));
});

server.listen(4000, () => console.log("Server running..."));
```

---

### HTTP Example
```js
const http = require("http");

http.createServer((req, res) => {
  let ab = new ArrayBuffer(3);
  let view = new Uint8Array(ab);
  view.set([65, 66, 67]); // 'ABC'

  res.writeHead(200, { "Content-Type": "application/octet-stream" });
  res.end(Buffer.from(ab)); // send raw binary
}).listen(3000, () => console.log("HTTP server running..."));
```

---

### WebSocket Example
```js
const WebSocket = require("ws");
const wss = new WebSocket.Server({ port: 8080 });

wss.on("connection", ws => {
  let ab = new ArrayBuffer(3);
  let view = new Uint8Array(ab);
  view.set([120, 121, 122]); // 'xyz'

  ws.send(ab); // WebSocket supports ArrayBuffer directly
});
```

---

## 5. Key Takeaways
- **Disk (fs)** → Convert ArrayBuffer → Node.js Buffer → write file.  
- **Network (TCP/HTTP)** → Convert ArrayBuffer → Buffer → send over socket/response.  
- **WebSocket** (browser/Node.js) → can directly send ArrayBuffer (no conversion needed).  
- This makes binary data portable between **memory, disk, and network**.  

---

## ✅ Summary
- ArrayBuffer = **in-memory binary data**.  
- To transfer:  
  - **Disk** → `fs.writeFile` with `Buffer.from(arrayBuffer)`.  
  - **Network** → `socket.write(Buffer.from(arrayBuffer))`.  
  - **WebSocket** → `ws.send(arrayBuffer)` directly.  
- Conversion between **ArrayBuffer ↔ Buffer** is the bridge between raw JS memory and Node.js I/O.
