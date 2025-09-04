# 📦 ArrayBuffer in JavaScript

---

## 🔹 1. What is an ArrayBuffer?
- An **ArrayBuffer** is a **fixed-length block of raw binary data**.  
- It doesn’t store numbers, strings, or objects directly → it stores **bytes** (like 0s and 1s).  
- It’s mainly used when you need to deal with **binary data** (files, images, network streams, etc.).

👉 Think of it as an **empty box of bytes** that you can fill with data using special "views".  

---

## 🔹 2. Why do we use ArrayBuffer?
JavaScript normally works with strings and objects, but sometimes we need **low-level control** over data:  
- Handling files (images, PDFs, videos).  
- Working with network protocols (WebSockets, binary streams).  
- Storing binary data in **IndexedDB** (browser database).  
- Performing cryptographic operations (hashing, encryption).  

Without ArrayBuffer, handling such **raw bytes** efficiently would be very difficult.

---

## 🔹 3. How do we use ArrayBuffer?

### Step 1: Create an ArrayBuffer
```js
// Create a buffer of 16 bytes
let buffer = new ArrayBuffer(16);
console.log(buffer.byteLength); // 16
```
👉 Here, `buffer` is just **16 empty bytes** (all `0` initially).  

---

### Step 2: Create a TypedArray View
We can’t directly read/write `ArrayBuffer`.  
Instead, we use **views** like `Uint8Array`, `Int16Array`, `Float32Array`, etc.

```js
let buffer = new ArrayBuffer(8);

// Create a 8-bit unsigned integer view
let view = new Uint8Array(buffer);

view[0] = 255;   // max value for Uint8
view[1] = 10;

console.log(view);       // Uint8Array(8) [255, 10, 0, 0, 0, 0, 0, 0]
console.log(buffer);     // ArrayBuffer(8) {}
```

---

### Step 3: DataView (more flexible view)
If you want to read/write **different types of numbers** (int, float, etc.) inside the same buffer → use `DataView`.

```js
let buffer = new ArrayBuffer(4);
let view = new DataView(buffer);

view.setInt16(0, 500);   // store 16-bit integer at byte 0
view.setInt8(2, 127);    // store 8-bit integer at byte 2

console.log(view.getInt16(0)); // 500
console.log(view.getInt8(2));  // 127
```

---

## 🔹 4. Real-World Examples

### 1. Working with Files (Browser File API)
```js
let fileInput = document.querySelector("input[type=file]");
fileInput.onchange = () => {
  let file = fileInput.files[0];
  let reader = new FileReader();

  reader.onload = (e) => {
    let buffer = e.target.result; // ArrayBuffer
    console.log(new Uint8Array(buffer)); // raw bytes
  };

  reader.readAsArrayBuffer(file);
};
```

### 2. Networking (WebSockets / Fetch binary data)
```js
fetch("image.png")
  .then(res => res.arrayBuffer())
  .then(buffer => {
    let bytes = new Uint8Array(buffer);
    console.log("Image first bytes:", bytes.slice(0, 10));
  });
```

---

## ✅ Summary
- `ArrayBuffer` = raw binary data container (can’t access directly).  
- **TypedArrays** (`Uint8Array`, `Float32Array`, etc.) = views for specific data types.  
- **DataView** = flexible view for multiple data types.  
- Used in **files, network, cryptography, databases, WebGL**.  

👉 At the end of the day, `ArrayBuffer` is how JavaScript deals with **binary data**, like how UTF-8 stored emojis in raw bytes.  
