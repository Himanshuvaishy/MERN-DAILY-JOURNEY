# 📦 Buffer in Node.js — Deep Dive

---

## 1) What is a Buffer?
- A `Buffer` is a fixed-size sequence of bytes (raw binary data).  
- It’s Node.js’s primary way to work with **binary data** (files, network packets, crypto, images, etc.).  
- Modern `Buffer` is a subclass of `Uint8Array` (so interoperable with `ArrayBuffer` / TypedArrays).

---

## 2) Why Buffers Exist
Use Buffers whenever you need to handle binary data:
- File I/O (fs.readFile, streams)
- Network sockets (TCP, HTTP)
- Cryptography (hashing, encryption)
- Binary protocols (WebSocket frames)
- Encoding/decoding base64/hex

---

## 3) Creating Buffers

```js
const { Buffer } = require('buffer');

// Allocated and zero-filled
const b1 = Buffer.alloc(10);

// Fast but uninitialized — must overwrite
const b2 = Buffer.allocUnsafe(10);

// From array of bytes
const b3 = Buffer.from([0x41, 0x42, 0x43]); // 'ABC'

// From string
const b4 = Buffer.from('Hello', 'utf8');

// From ArrayBuffer
const ab = new ArrayBuffer(4);
const b5 = Buffer.from(ab);

// Copy from another buffer
const b6 = Buffer.from(b4);
```

---

## 4) Buffer Properties & Helpers

```js
Buffer.isBuffer(obj);
Buffer.byteLength("Hello", "utf8");
Buffer.concat([buf1, buf2], totalLength);
```

- `buf.length` → number of bytes in buffer.

---

## 5) Buffer <-> ArrayBuffer <-> TypedArray

```js
// Buffer → ArrayBuffer
const ab = buf.buffer.slice(buf.byteOffset, buf.byteOffset + buf.byteLength);

// ArrayBuffer → Buffer
const b = Buffer.from(ab);

// Buffer → Uint8Array
const u8 = new Uint8Array(buf.buffer, buf.byteOffset, buf.length);
```

---

## 6) Encoding & Strings

```js
const buf = Buffer.from("नमस्ते", "utf8");
console.log(buf.toString("utf8"));   // नमस्ते
console.log(buf.toString("hex"));    // hex representation
```

- Supports: utf8, ascii, latin1, base64, hex, utf16le.

---

## 7) Reading & Writing Numeric Values

```js
// Unsigned
buf.writeUInt16LE(0x1234, 0);
console.log(buf.readUInt16LE(0));

// Signed
buf.writeInt8(-1, 0);
console.log(buf.readInt8(0));

// Float / Double
buf.writeFloatLE(3.14, 0);
buf.writeDoubleBE(Math.PI, 0);
```

- `LE` = Little Endian, `BE` = Big Endian.

---

## 8) Common Buffer Methods

```js
buf.toString("utf8", 0, buf.length);
buf.write("Hi", 0, "utf8");
buf.copy(targetBuf, targetStart, sourceStart, sourceEnd);
buf.slice(start, end); // shares memory
buf.fill(0xff);
Buffer.concat([buf1, buf2]);
buf.equals(otherBuf);
```

⚠️ `slice()` shares memory (mutations affect both).

---

## 9) Buffers with Streams

```js
const fs = require("fs");
const rs = fs.createReadStream("file.bin");
rs.on("data", chunk => {
  console.log("Got chunk:", chunk.length);
});
```

- Streams give chunks as Buffers.

---

## 10) Buffers with Crypto

```js
const crypto = require("crypto");
const hash = crypto.createHash("sha256")
  .update(Buffer.from("hello"))
  .digest("hex");
console.log(hash);
```

---

## 11) Performance Notes
- `Buffer.allocUnsafe()` is faster but **uninitialized** (must overwrite).  
- `slice()` is zero-copy (cheap).  
- `copy()` makes a real copy (expensive).  
- Use `Buffer.concat` for joining multiple buffers.

---

## 12) Safety Best Practices
- Avoid sending uninitialized memory (`allocUnsafe`).  
- Use `StringDecoder` for UTF-8 boundaries in streams.  
- Always specify encoding (`utf8`, `hex`, etc.).

---

## 13) Recipes

**Write to file**
```js
fs.writeFileSync("hello.txt", Buffer.from("Hello Node", "utf8"));
```

**Read file**
```js
const buf = fs.readFileSync("hello.txt");
console.log(buf.toString());
```

**Write multi-byte int**
```js
const buf = Buffer.alloc(4);
buf.writeUInt32LE(0xdeadbeef, 0);
console.log(buf.readUInt32LE(0).toString(16));
```

---

## 14) Buffer vs TypedArray vs DataView

| Feature        | Buffer (Node.js) | TypedArray | DataView |
|----------------|------------------|------------|----------|
| Ease of use    | High             | Medium     | Low (manual get/set) |
| Endianness     | Default LE/BE    | System default | Explicit LE/BE |
| Use case       | Node I/O, files  | Numeric ops | Binary protocols |

---

## ✅ Summary
- `Buffer` = Node.js tool for binary data.  
- Works with files, streams, crypto, networking.  
- Interoperable with `ArrayBuffer`, `TypedArray`, `DataView`.  
- Always watch encoding, memory safety, and slice vs copy.
