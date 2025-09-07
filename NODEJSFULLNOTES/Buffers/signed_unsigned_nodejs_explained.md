# 🔹 Signed vs Unsigned Values in JavaScript & Node.js

---

## 1. What are Signed and Unsigned Values?
- **Unsigned Values**  
  - Can only represent **non-negative numbers** (0 and positive).  
  - All bits are used for the number.  
  - Example: `Uint8` (Unsigned 8-bit Integer) → range **0 to 255**.

- **Signed Values**  
  - Can represent both **negative and positive numbers**.  
  - One bit (most significant bit = MSB) is used to store the **sign**.  
  - Example: `Int8` (Signed 8-bit Integer) → range **-128 to +127**.

---

## 2. Example with 8 Bits
| Representation       | Range       | Example Values |
|----------------------|-------------|----------------|
| **Unsigned (Uint8)** | 0 → 255     | 00000000 = 0, 11111111 = 255 |
| **Signed (Int8)**    | -128 → +127 | 00000000 = 0, 01111111 = 127, 10000000 = -128 |

👉 **Why difference?**  
Because signed numbers use **Two’s Complement** representation for negatives.

---

## 3. Two’s Complement (How Signed Numbers Work)
To represent a negative number in binary:  
1. Write the positive number in binary.  
2. Invert all bits (flip 0 ↔ 1).  
3. Add `1` to the result.  

Example: Represent `-5` in 8-bit signed (Int8):
1. +5 → `00000101`  
2. Invert → `11111010`  
3. Add 1 → `11111011`  

So, `-5` = `11111011` in signed 8-bit.

---

## 4. In JavaScript: TypedArrays
```js
let buffer = new ArrayBuffer(2);

// Unsigned 8-bit view
let u8 = new Uint8Array(buffer);
u8[0] = 255;
console.log(u8[0]); // 255

// Signed 8-bit view
let i8 = new Int8Array(buffer);
i8[0] = -1;
console.log(i8[0]); // -1
```

- `Uint8` → values 0–255  
- `Int8` → values -128–127  

---

## 5. Real-World Use Cases
- **Unsigned** → Images (pixel values 0–255), networking, binary files.  
- **Signed** → Math operations, physics simulations, financial calculations.  

---

## ✅ Summary (Basic)
- **Unsigned**: Only 0 and positive, full range of bits used for magnitude.  
- **Signed**: Includes negatives, one bit reserved for sign (Two’s Complement).  
- In JS, use `Int8Array`, `Int16Array`, `Int32Array` for signed, and `Uint8Array`, `Uint16Array`, `Uint32Array` for unsigned.  

---

## 🔹 Visual Bit Layout
### Unsigned (Uint8)
```
[0][0][0][0][0][0][0][1] = 1
[1][1][1][1][1][1][1][1] = 255
```

### Signed (Int8, Two’s Complement)
```
[0][1][1][1][1][1][1][1] = +127
[1][0][0][0][0][0][0][0] = -128
[1][1][1][1][1][1][1][1] = -1
```

---

# 🔹 How Node.js Identifies Signed vs Unsigned Values

---

## 1. Node.js Numbers Are Always Double (by default)
- In plain JavaScript, all numbers are stored as **64-bit floating-point (IEEE 754)**.  
- There is no distinction between signed/unsigned when you just write:
  ```js
  let x = 255;   // Just a number, no signed/unsigned concept
  ```

👉 Signed vs Unsigned only matters when you deal with **raw binary data** (like buffers, ArrayBuffer, files, networking).

---

## 2. Using TypedArrays
TypedArrays tell Node.js **how to interpret bytes**:

```js
let buffer = new ArrayBuffer(1);

// Unsigned 8-bit
let u8 = new Uint8Array(buffer);
u8[0] = 255;
console.log(u8[0]); // 255

// Signed 8-bit
let i8 = new Int8Array(buffer);
i8[0] = -1;
console.log(i8[0]); // -1
```

👉 Both use the same **raw bits (11111111)** but are **interpreted differently**:
- `Uint8Array` → 255  
- `Int8Array` → -1 (Two’s Complement)

---

## 3. Using Node.js Buffer
Node.js `Buffer` class has **explicit methods** to decide signed vs unsigned.

```js
const buf = Buffer.alloc(1);

// Unsigned write
buf.writeUInt8(255, 0);
console.log(buf.readUInt8(0)); // 255

// Signed write
buf.writeInt8(-1, 0);
console.log(buf.readInt8(0)); // -1
```

⚡ Key point:  
- `writeUInt8` → treat byte as unsigned  
- `writeInt8` → treat byte as signed  

So **you tell Node.js** how to interpret those bits.

---

## 4. DataView (More Flexible)
When working with ArrayBuffer + DataView, you also specify signed or unsigned:

```js
let buffer = new ArrayBuffer(1);
let view = new DataView(buffer);

// Unsigned
view.setUint8(0, 255);
console.log(view.getUint8(0)); // 255

// Signed
view.setInt8(0, -1);
console.log(view.getInt8(0));  // -1
```

👉 Same 8 bits in memory, but interpreted differently based on method.

---

## ✅ Summary (Node.js)
- Node.js **doesn’t guess** signed/unsigned → **you must choose**.  
- `Int8Array`, `Int16Array`, `Buffer.writeInt8()` → signed.  
- `Uint8Array`, `Uint16Array`, `Buffer.writeUInt8()` → unsigned.  
- Under the hood: the **raw binary bits are the same**; only interpretation changes.  

---

👉 In short: **Signed vs Unsigned is all about interpretation. Node.js gives you tools (TypedArrays, Buffer, DataView) to choose how you want to read/write those raw bytes.**
