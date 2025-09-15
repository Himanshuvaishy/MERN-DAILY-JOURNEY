# 📘 Base-64 Encoding in Node.js

---

## 🔹 What is Base-64 Encoding?

- **Base-64** is a way to represent **binary data (bytes)** in **text form** using only **64 characters**:
  ```
  A–Z, a–z, 0–9, +, /
  ```
  and `=` as padding.

- Each **3 bytes (24 bits)** of binary data are split into **4 groups of 6 bits**, then mapped to the 64-character alphabet.

### Example
- String: `"Man"`  
- ASCII Bytes: `77 97 110` → Binary: `01001101 01100001 01101110`  
- Group into 6-bit chunks: `010011 010110 000101 101110`  
- Convert to Base64 Alphabet: `TWFu`  

✅ `"Man"` → `"TWFu"` in Base-64

---

## 🔹 Why Do We Use Base-64?

1. **Safe Transmission**  
   - Some channels (emails, URLs, JSON, XML) can’t handle raw binary.  
   - Base-64 makes it safe because it’s **pure text**.

2. **Binary-to-Text Encoding**  
   - For storing or transmitting images, audio, PDFs as text.

3. **Common Use Cases**  
   - Sending attachments in emails.  
   - Storing small images in JSON or HTML.  
   - HTTP Basic Auth (`Authorization: Basic <Base64>`).  
   - JWT (JSON Web Tokens) headers/payloads.  

---

## 🔹 Base-64 in Node.js

Node.js `Buffer` provides built-in support for Base-64.

### ✅ Encoding a String to Base-64
```js
const str = "Hello Himanshu!";
const buf = Buffer.from(str, "utf8"); // create buffer from string
const base64 = buf.toString("base64");
console.log(base64); 
// Output: SGVsbG8gSGltYW5zaHUh
```

---

### ✅ Decoding Base-64 Back to String
```js
const decoded = Buffer.from(base64, "base64").toString("utf8");
console.log(decoded); 
// Output: Hello Himanshu!
```

---

### ✅ Encoding Binary Files
```js
const fs = require("fs");

// Read image as buffer
const image = fs.readFileSync("photo.png");

// Convert to Base-64 string
const base64Img = image.toString("base64");
console.log(base64Img);

// Save back to file
fs.writeFileSync("copy.png", Buffer.from(base64Img, "base64"));
```

---

## 🔹 Limitations of Base-64

- Increases size by **~33%** (every 3 bytes become 4 chars).  
- Not efficient for very large files → better to send as raw binary (streaming).  
- Should be used mainly for **short data embedding** (tokens, small media, metadata).  

---

## ✅ Summary

- **Base-64** is a way to represent binary data as safe text.  
- Used in **emails, tokens, file embedding, networking**.  
- Node.js Buffers make encoding/decoding **super easy** with `.toString("base64")` and `Buffer.from(..., "base64")`.  
- Best for **small data**, avoid for large files (use streams instead).  
