# 📘 Binary Data in Node.js — Master Notes

This document is a full guide covering **Buffer, ArrayBuffer, TypedArray, DataView, UTF-8 Encoding, Endianness, Signed vs Unsigned values, Multi-byte storage, Transferring Data, and Buffer memory management**.  
It also includes **why we study these topics** and their **real-world importance**.

---

# 1. Why Do We Study Buffers & Binary Data?

Most JavaScript beginners only see **strings, numbers, JSON**.  
But real-world Node.js applications deal with **raw binary data** like:
- Images, audio, video
- PDFs, ZIP files, executables
- Network packets (HTTP, TCP, WebSocket)
- Cryptography (hashes, encryption)
- Databases & IoT protocols

👉 Buffers + related tools are **the bridge between JS and system-level data**.

---

# 2. Buffer in Node.js

## What is a Buffer?
- A `Buffer` is a fixed-size sequence of bytes (raw memory).  
- It allows Node.js to handle **binary streams** of data.  
- Subclass of `Uint8Array` (shares memory model with `ArrayBuffer`).

### Creating Buffers
```js
const { Buffer } = require('buffer');

const b1 = Buffer.alloc(10);              // zero-filled
const b2 = Buffer.allocUnsafe(10);        // uninitialized (faster, unsafe)
const b3 = Buffer.from([0x41, 0x42, 0x43]); // from bytes ('ABC')
const b4 = Buffer.from('Hello', 'utf8');  // from string
```

### Common Uses
- File I/O (`fs.readFile` gives Buffer)
- Network packets (`socket.on('data')` gives Buffer)
- Crypto (`crypto.update(Buffer.from(...))`)
- Converting between encodings (`utf8`, `hex`, `base64`)

---

# 3. Encoding (ASCII, Unicode, UTF-8)

- **ASCII** → 1 byte, 0–127 only.
- **Unicode** → Defines characters, not encoding.
- **UTF-8** → Variable-length encoding (1–4 bytes per character).

| Character | Unicode | UTF-8 Bytes |
|-----------|----------|--------------|
| A         | U+0041   | 0x41 |
| é         | U+00E9   | 0xC3 0xA9 |
| नमस्ते     | U+0928.. | multiple 3-byte sequences |
| 😊        | U+1F60A  | 0xF0 0x9F 0x98 0x8A |

👉 UTF-8 is **backward compatible with ASCII** and the web standard.

---

# 4. Signed vs Unsigned Integers

- **Unsigned**: All bits store value. Example: `11111111` = 255.  
- **Signed (Two’s complement)**: MSB is sign bit. Example: `11111111` = -1.  

Node.js Buffers support both via `readUInt8` / `readInt8`, etc.

```js
const buf = Buffer.alloc(1);
buf.writeInt8(-1, 0);
console.log(buf.readUInt8(0)); // 255
```

---

# 5. Endianness (Byte Order)

- **Little Endian (LE)**: least significant byte first (Intel, AMD).  
- **Big Endian (BE)**: most significant byte first (network order).  

Example: `0x1234`

| Address | LE Value | BE Value |
|---------|----------|----------|
| 0x00    | 0x34     | 0x12 |
| 0x01    | 0x12     | 0x34 |

👉 Node.js gives methods like `readUInt16LE` and `readUInt16BE`.

---

# 6. Multi-Byte Storage & Typed Arrays

- Numbers larger than 255 need multiple bytes.  
- **Typed Arrays** are views over `ArrayBuffer` optimized for performance.

### Example
```js
const ab = new ArrayBuffer(4);
const u32 = new Uint32Array(ab);
u32[0] = 123456;
console.log(u32[0]); // 123456
```

### DataView
```js
const dv = new DataView(ab);
dv.setUint16(0, 500, true);   // Little Endian
console.log(dv.getUint16(0, true)); // 500
```

- Use **TypedArray** for performance.  
- Use **DataView** for low-level parsing with explicit endianness.

---

# 7. Transferring Data (Memory → Disk & Network)

### Writing ArrayBuffer to Disk
```js
const fs = require("fs");
let ab = new ArrayBuffer(2);
let view = new Uint8Array(ab);
view[0] = 72; view[1] = 105; // "Hi"
fs.writeFileSync("hello.bin", Buffer.from(ab));
```

### Sending Over TCP
```js
const net = require("net");
const server = net.createServer(socket => {
  let ab = new ArrayBuffer(4);
  new Uint8Array(ab).set([1,2,3,4]);
  socket.write(Buffer.from(ab));
});
server.listen(4000);
```

### Sending Over WebSocket
```js
ws.send(arrayBuffer); // supported directly
```

---

# 8. Alloc vs AllocUnsafe & Buffer Pool

## `Buffer.alloc(size)`
- Zero-fills memory.  
- Safe but slower.  
- Recommended.

## `Buffer.allocUnsafe(size)`
- No initialization (may contain old memory).  
- Very fast.  
- Must overwrite before use.  
- Dangerous if exposed to clients.

## Buffer Pool
- Node.js maintains a slab (~8 KB) for small Buffers.  
- Many small allocations reuse this pool.  
- Large Buffers (>8 KB) allocated separately.  

```js
const b1 = Buffer.alloc(1000); // from pool
const b2 = Buffer.alloc(9000); // separate allocation
```

---

# 9. Real-World Importance

- **File Upload/Download** → Buffers handle binary chunks.  
- **Streaming Video/Audio** → Buffers stream data efficiently.  
- **Chat Apps** → Buffers store emojis + media.  
- **Security** → Crypto APIs use Buffers.  
- **IoT** → Buffers decode binary packets from sensors.  

---

# ✅ Final Summary

We study Buffers & binary data in Node.js because they enable:
- Handling **non-textual data** (files, media, protocols).  
- Working with **memory-level details** (signed/unsigned, endianness, encoding).  
- Building **real servers** that process binary streams securely & efficiently.  

👉 Buffers turn JavaScript from just a **web scripting language** into a **system-level language** for networking, files, and crypto.
