
# ✅ 7. What are Callbacks and Promises in Node.js?

---

### 🔹 1. What (Simple Definition)

- A **callback** is a function passed into another function as an argument and is **executed after** the first function completes.
- A **promise** is an object that represents the **future result** of an asynchronous operation — either **resolved** (success) or **rejected** (error).

---

### 🔹 2. Why (Purpose / Importance)

Node.js is **asynchronous and non-blocking** by nature, so it needs ways to handle code that takes time to complete (like fetching data or reading a file).  
- **Callbacks** were the traditional way to deal with this.
- **Promises** (and `async/await`) were introduced to make asynchronous code **cleaner, readable, and maintainable**.

---

### 🔹 3. How (Detailed Explanation)

#### ✅ Callback Example

```js
function fetchData(callback) {
  setTimeout(() => {
    callback("Data fetched!");
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});
```

🟢 **Output:**
```
Data fetched!
```

#### ❌ Problem with Callbacks: “Callback Hell”

```js
login(user, (err, res) => {
  getProfile(res.id, (err2, profile) => {
    getPosts(profile.id, (err3, posts) => {
      // and so on...
    });
  });
});
```

Hard to read and debug — this is **callback hell**.

---

#### ✅ Promise Example

```js
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Data fetched via promise!");
    }, 1000);
  });
}

fetchData()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.error(err);
  });
```

🟢 **Output:**
```
Data fetched via promise!
```

---

#### ✅ Async/Await (Modern way using Promises)

```js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Data fetched via async/await!"), 1000);
  });
}

async function main() {
  const result = await fetchData();
  console.log(result);
}

main();
```

🟢 **Output:**
```
Data fetched via async/await!
```

---

### 🔹 Key Differences Table

| Feature        | Callback                     | Promise                          |
|----------------|-------------------------------|----------------------------------|
| Syntax         | Function parameter            | Object with `.then()`/`.catch()` |
| Error Handling | Try-catch hard to manage      | Easier with `.catch()` or `try`  |
| Readability    | Can lead to "callback hell"   | Clean and chainable              |
| Flexibility    | Limited                       | Better composition of tasks      |

---

### 🔹 4. Real-World Analogy

- **Callback**: You call your friend and ask him to remind you. When he does, that’s the callback.
- **Promise**: You order something online and receive a **promise (receipt)**. You continue working. Once delivered (fulfilled), you act on it.

---

### 🔹 5. Interview-Ready Summary

Node.js uses **callbacks** and **promises** to handle asynchronous code.  
- Callbacks are simple but lead to **callback hell**.
- Promises (and `async/await`) make code **cleaner, readable, and easier to debug**.

---

### 💡 Tip

Use `async/await` in real projects and interviews — it's modern, readable, and easier to handle errors with `try/catch`.

---

### ⚠️ Trap

Don’t **mix callbacks and promises** in the same logic — it can create bugs and make the code hard to maintain.
