# 🔹 Writing Multi-Byte Data in Node.js & JavaScript

---

## 1. What is Multi-Byte Data?
- **Multi-byte data** means numbers or values that require **more than 1 byte (8 bits)** to store.  
- Examples:  
  - 8-bit (1 byte) → 0 to 255 (`Uint8`)  
  - 16-bit (2 bytes) → 0 to 65535 (`Uint16`)  
  - 32-bit (4 bytes) → up to ~4 billion (`Uint32`)  
  - 64-bit (8 bytes) → very large numbers (`BigInt`, `Float64`)  

---

## 2. Endianness: How Multi-Byte Numbers Are Stored
Multi-byte numbers can be stored in memory in **two ways**:

### 🔹 Big-Endian (BE)
- **Most Significant Byte (MSB)** stored first.  
- Human-friendly, used in networking (“network byte order”).  

Example (16-bit number = `0x1234`):  
```
Address:  0x00   0x01
Value:    0x12   0x34
```

### 🔹 Little-Endian (LE)
- **Least Significant Byte (LSB)** stored first.  
- Default in most CPUs (Intel, AMD), and Node.js.  

Example (same number `0x1234`):  
```
Address:  0x00   0x01
Value:    0x34   0x12
```

👉 Endianness matters when **sharing binary data across systems**.

---

## 3. Writing Multi-Byte Data in Node.js Buffers

Node.js `Buffer` has explicit methods for writing signed/unsigned multi-byte data:

```js
const buf = Buffer.alloc(4);

// Writing Unsigned 16-bit (2 bytes)
buf.writeUInt16LE(0x1234, 0); // Little Endian
buf.writeUInt16BE(0x1234, 2); // Big Endian

console.log(buf);
// <Buffer 34 12 12 34>
```

### Breakdown:
- First two bytes → `34 12` (Little-Endian)  
- Last two bytes → `12 34` (Big-Endian)  

---

## 4. Supported Buffer Methods
- `writeUInt16LE`, `writeUInt16BE`  
- `writeInt16LE`, `writeInt16BE`  
- `writeUInt32LE`, `writeUInt32BE`  
- `writeInt32LE`, `writeInt32BE`  
- `writeFloatLE`, `writeFloatBE`  
- `writeDoubleLE`, `writeDoubleBE`  

👉 `LE` = Little Endian, `BE` = Big Endian.

---

## 5. Reading Multi-Byte Data from Buffer
Use corresponding `read*` methods:

```js
console.log(buf.readUInt16LE(0)); // 4660 (0x1234)
console.log(buf.readUInt16BE(2)); // 4660 (0x1234)
```

---

## 6. Writing Multi-Byte Data with DataView

Using `ArrayBuffer + DataView` also supports endian control:

```js
let buffer = new ArrayBuffer(4);
let view = new DataView(buffer);

view.setUint16(0, 0x1234, true);  // Little Endian
view.setUint16(2, 0x1234, false); // Big Endian

console.log(view.getUint16(0, true));  // 4660
console.log(view.getUint16(2, false)); // 4660
```

---

## ✅ Summary
- Multi-byte data = values stored across multiple bytes.  
- **Endianness (LE vs BE)** determines byte order.  
- Node.js `Buffer` and `DataView` give explicit control with LE/BE methods.  
- Always specify endianness when working with **binary protocols, files, or networking**.  

---

👉 In short: **Writing multi-byte data is all about correctly choosing signed/unsigned and endianness (LE/BE) when storing or reading binary values.**
