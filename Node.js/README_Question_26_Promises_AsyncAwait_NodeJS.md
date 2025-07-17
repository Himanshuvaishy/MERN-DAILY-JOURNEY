# ✅ 26. What are Promises and async/await in Node.js?

---

### 🔹 1. What (Simple Definition)

#### ✅ Promises:
A **Promise** is an object representing the **eventual completion or failure** of an asynchronous operation.

> It lets you attach **handlers** to deal with success (`.then()`) or failure (`.catch()`).

---

#### ✅ async/await:
`async` and `await` are **syntax sugar** introduced in ES2017 to work with Promises in a **cleaner and more readable way**.

---

### 🔹 2. Why (Purpose / Importance)

- Replaces **callback hell**
- Improves **code readability**
- Makes asynchronous code look and behave more like **synchronous code**
- Easier to debug

---

### 🔹 3. How (Examples)

#### ✅ Basic Promise

```js
const getData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data received!');
    }, 2000);
  });
};

getData()
  .then(data => console.log(data))
  .catch(err => console.error(err));

✅ Using async/await with the same function
js
Copy
Edit
const getData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data received!');
    }, 2000);
  });
};

async function fetchData() {
  try {
    const result = await getData();
    console.log(result);
  } catch (err) {
    console.error(err);
  }
}

fetchData();
🔹 4. Real-World Analogy
A Promise is like ordering food online.
You place an order (promise), and then wait for it to be delivered (then).
If there's an issue (catch), you handle it (like asking for a refund).

🔹 5. Interview-Ready Summary
Concept	Description
Promise	Object that handles async operations
.then()	Executes on success
.catch()	Executes on failure
async	Marks a function to use await inside
await	Waits for a Promise to resolve/reject

💡 Tip
You can only use await inside async functions.

Mixing Promises and callbacks in the same code is not recommended.

⚠️ Trap
Don’t forget to use try...catch when using await to handle errors.

Using too many awaits in sequence can slow down performance if they can run in parallel.