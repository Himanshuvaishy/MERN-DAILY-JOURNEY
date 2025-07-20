# 34. Blocking vs Non-Blocking Code in Node.js

---

## 🔍 What

In Node.js, the difference between **blocking** and **non-blocking** code is how it handles **I/O operations** like file reading, database access, or network requests.

| Term           | Description                                     |
|----------------|-------------------------------------------------|
| **Blocking**   | Code that **waits** for the operation to finish |
| **Non-blocking**| Code that **continues running** immediately and uses a callback or Promise for the result later |

---

## 💡 Why It Matters

Node.js is **single-threaded**, so if a task **blocks**, it stops everything else.

To keep the app responsive and scalable, we use **non-blocking code**.

---

## ⚙️ How They Work (with Examples)

---

### 🔁 Blocking Example (Synchronous)

```js
const fs = require('fs');

const data = fs.readFileSync('file.txt', 'utf-8');
console.log(data); // This line waits until file is read
console.log('Done');
🟥 If file.txt is large or the disk is slow, this will freeze the event loop.

🔁 Non-Blocking Example (Asynchronous)
js
Copy
Edit
const fs = require('fs');

fs.readFile('file.txt', 'utf-8', (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});

console.log('Done'); // Runs immediately without waiting
🟩 The file is read in the background, and the app continues.

🧠 Real-World Analogy
Blocking: Standing in line at a bank until the person ahead is done

Non-Blocking: Taking a token and sitting while the staff calls you when ready

📦 Common Examples
Operation	Blocking Version	Non-Blocking Version
File Read (Sync)	fs.readFileSync()	fs.readFile()
Database Query	Not available	Use async/await or Promises
HTTP Request	Not available	http.get() or axios()

✅ Best Practices
Use non-blocking APIs whenever possible

Use Promises or async/await for cleaner non-blocking code

Offload CPU-heavy tasks using worker threads or child processes

📦 Summary
Feature	            Blocking	Non-Blocking
Waits for result    	✅ Yes	❌ No
Blocks event loop	    ✅ Yes	❌ No
Recommended?	        ❌ No (for I/O tasks)	✅ Yes
Syntax style	      Usually synchronous	Callback / Promise / async