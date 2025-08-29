# 📝 Writing and Appending Files in Node.js (fs Module)

The **`fs` (File System)** module in Node.js provides methods to interact with the file system.  
Two important methods are **`writeFile`** (or `writeFileSync`) and **`appendFile`** (or `appendFileSync`).  

---

## 🔹 1. `fs.writeFile`

### Description:
- Creates a new file **or overwrites** an existing file with the given content.
- Asynchronous by default (non-blocking).

### Syntax:
```js
const fs = require('fs');

fs.writeFile('file.txt', 'Hello Himanshu!', (err) => {
  if (err) throw err;
  console.log('File created or overwritten!');
});
```

### Synchronous version:
```js
fs.writeFileSync('file.txt', 'Hello Himanshu!');
console.log('File created or overwritten!');
```

### Key Points:
- If the file exists → content will be **replaced** (overwritten).  
- If the file does not exist → a **new file** will be created.  
- Encoding defaults to `"utf8"`.  

---

## 🔹 2. `fs.appendFile`

### Description:
- Adds (appends) content at the **end of an existing file**.  
- If the file does not exist, it will create a new file.

### Syntax:
```js
fs.appendFile('file.txt', '\nThis is appended text.', (err) => {
  if (err) throw err;
  console.log('Content appended!');
});
```

### Synchronous version:
```js
fs.appendFileSync('file.txt', '\nThis is appended text.');
console.log('Content appended!');
```

### Key Points:
- If the file exists → new content is **added at the end**.  
- If the file does not exist → a new file is created with that content.  
- Good for logging or adding history data.  

---

## 🔹 Example Combined Usage

```js
const fs = require('fs');

// Step 1: Write a new file (overwrite if exists)
fs.writeFileSync('demo.txt', 'This is the first line.');

// Step 2: Append content to the same file
fs.appendFileSync('demo.txt', '\nThis is the second line.');

// Step 3: Read file to verify
const data = fs.readFileSync('demo.txt', 'utf8');
console.log(data);
```

### Output:
```
This is the first line.
This is the second line.
```

---

## ✅ Summary Table

| Method            | Behavior                                | Creates File if Missing | Overwrites File? |
|-------------------|----------------------------------------|--------------------------|------------------|
| `fs.writeFile`    | Write content to file (async)          | ✅ Yes                   | ✅ Yes           |
| `fs.writeFileSync`| Write content synchronously            | ✅ Yes                   | ✅ Yes           |
| `fs.appendFile`   | Append content to file (async)         | ✅ Yes                   | ❌ No (appends)  |
| `fs.appendFileSync`| Append content synchronously          | ✅ Yes                   | ❌ No (appends)  |

---

## 🚀 When to Use?
- **`writeFile`** → when you want to create/overwrite a file completely.  
- **`appendFile`** → when you want to keep adding data without deleting existing content.  
