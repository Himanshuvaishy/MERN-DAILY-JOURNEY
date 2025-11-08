# Why We Await Twice in a fetch() Call

## 🧠 Concept
When we use `fetch()` in JavaScript, we often see two `await` statements.

This happens because:
1. **First await** waits for **response headers**.
2. **Second await** waits for **response body (data)**.

---

## ⚙️ Example
```js
const res = await fetch("https://example.com");
// ✅ First await: waits until response headers are received

const data = await res.json();
// ✅ Second await: waits until full response body is downloaded and parsed
```

---

## 📦 Step-by-Step

| Step | What Happens | Await |
|------|----------------|--------|
| 1️⃣ | Request sent to server | — |
| 2️⃣ | Server sends back headers | `await fetch()` resolves |
| 3️⃣ | Response body starts streaming | — |
| 4️⃣ | Body fully received | `await res.json()` resolves |

---

## 🧩 In Simple Words
- First `await` → “Server has responded!” (headers ready)
- Second `await` → “All data downloaded and ready to use.”

---

## 🧠 Real Example
```js
const res = await fetch("https://jsonplaceholder.typicode.com/users/1");
console.log("Headers received ✅");

const data = await res.json();
console.log("Data received ✅", data);
```

Output:
```
Headers received ✅
Data received ✅ { id: 1, name: 'Leanne Graham', ... }
```

---

## ✅ Summary

| Await | Waits For | Example |
|--------|------------|----------|
| 1️⃣ `await fetch()` | Response headers | `Response` object |
| 2️⃣ `await res.json()` | Full body + parse | Final data |

---

**Created by:** GPT-5  
**Topic:** Why We Await Twice in fetch()  
**Date:** 2025
