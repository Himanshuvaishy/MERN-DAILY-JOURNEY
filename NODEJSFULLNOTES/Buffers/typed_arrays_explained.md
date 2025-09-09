# 🎯 Typed Arrays in JavaScript

Typed Arrays give us a way to work directly with **binary data** using special array-like structures.  
They’re **optimized, fast, and backed by `ArrayBuffer`**.

---

## 🧱 What Are Typed Arrays?
- Typed Arrays are **specialized views** over `ArrayBuffer` for specific data types.  
- **Difference from DataView**:  
  - `DataView` → Fine-grained control (signed/unsigned, endianness).  
  - **TypedArray** → Optimized, high-level for a specific type.  
- Each Typed Array has its **own constructor** (`Int8Array`, `Float64Array`, etc.).  

---

## 🧪 List of Typed Arrays

| Category           | Types |
|--------------------|-----------------------------------------------|
| **Signed Integers**   | `Int8Array`, `Int16Array`, `Int32Array`, `BigInt64Array` |
| **Unsigned Integers** | `Uint8Array`, `Uint16Array`, `Uint32Array`, `BigUint64Array` |
| **Clamped Integers**  | `Uint8ClampedArray` |
| **Floating Point**    | `Float32Array`, `Float64Array` |

---

## 🧮 Size Representation
- **8** → 1 byte per element  
- **16** → 2 bytes per element  
- **32** → 4 bytes per element  
- **64** → 8 bytes per element  

---

## 🛠️ Creating Typed Arrays

```js
// 1️⃣ Using an existing ArrayBuffer
const buf = new ArrayBuffer(4);
const u8 = new Uint8Array(buf);

// 2️⃣ Create with size (zero-filled)
const u16 = new Uint16Array(2); // 2 elements, 4 bytes total

// 3️⃣ Create with initial values
const u8 = new Uint8Array([0xfe, 0x53, 0xde, 0x99]);

// 4️⃣ Create and fill
const u8 = new Uint8Array(4).fill(7); // [7, 7, 7, 7]
```

---

## ✍️ Reading & Writing with Typed Arrays
```js
const buf = new ArrayBuffer(4);
const u16 = new Uint16Array(buf);

// Write values
u16[0] = 500;
u16[1] = 1000;

// Read values
console.log(u16[0]); // 500
console.log(u16[1]); // 1000
```

👉 TypedArrays behave like normal arrays but enforce type.

---

## ✍️ Reading & Writing with DataView
```js
const buf = new ArrayBuffer(4);
const view = new DataView(buf);

// Write values (explicit control of signed/unsigned + endianness)
view.setUint16(0, 500, true);   // Little Endian
view.setInt16(2, -500, false);  // Big Endian

// Read values
console.log(view.getUint16(0, true));   // 500
console.log(view.getInt16(2, false));   // -500
```

👉 `DataView` gives **full control** over:
- Signed vs Unsigned  
- Endianness (Little vs Big Endian)

---

## ⚖️ Difference: DataView vs TypedArray

| Feature             | TypedArray                          | DataView |
|----------------------|-------------------------------------|----------|
| **Ease of Use**      | Simple, array-like interface        | More verbose (get/set methods) |
| **Data Types**       | One fixed type (e.g., `Uint8Array`) | Supports all numeric types |
| **Endianness**       | Uses system endianness (usually LE) | Can specify LE or BE explicitly |
| **Use Case**         | Performance + typed data processing | Low-level binary parsing/control |

---

## 🧩 ArrayBuffer Extra Features
```js
// Resizable buffer
const a = new ArrayBuffer(4, { maxByteLength: 10 }); 

// Detached buffer (after transfer)
const b = a.transfer(); // Now `a` is empty (detached)
```

- **maxByteLength** → Maximum size for resizable buffers.  
- **Resizable** → Default is `false`, enabled if `maxByteLength` is set.  
- **Detached** → If transferred, the buffer is emptied (cannot be used).  

---

## 📌 Summary
- **TypedArrays** → Best for performance and type-specific operations.  
- **DataView** → Best for fine-grained control (endianness + mixed types).  
- Both share the same **ArrayBuffer**.  
- Reading/writing differs:  
  - `TypedArray[index] = value`  
  - `DataView.setUint16(offset, value, littleEndian)`  

---

👉 In short: **Use DataView when you need control, use TypedArray when you need speed.**
