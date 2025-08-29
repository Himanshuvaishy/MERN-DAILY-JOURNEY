# 📂 Node.js File Operations (`fs` Module)

The **`fs` (File System)** module in Node.js allows you to interact with the file system.  
Apart from reading/writing/appending files, you can also **rename, delete, move, and copy files**.  

---

## 🔹 1. Renaming Files

### Asynchronous:
```js
const fs = require('fs');

fs.rename('old.txt', 'new.txt', (err) => {
  if (err) throw err;
  console.log('File renamed!');
});
```

### Synchronous:
```js
fs.renameSync('old.txt', 'new.txt');
console.log('File renamed!');
```

---

## 🔹 2. Deleting Files

### Asynchronous:
```js
fs.unlink('file.txt', (err) => {
  if (err) throw err;
  console.log('File deleted!');
});
```

### Synchronous:
```js
fs.unlinkSync('file.txt');
console.log('File deleted!');
```

---

## 🔹 3. Moving Files (Rename with Path Change)

In Node.js, moving a file = renaming it with a new directory path.

```js
fs.rename('file.txt', './backup/file.txt', (err) => {
  if (err) throw err;
  console.log('File moved!');
});
```

✅ It moves the file into `backup/` folder.  

---

## 🔹 4. Copying Files

### Asynchronous:
```js
fs.copyFile('file.txt', 'copy.txt', (err) => {
  if (err) throw err;
  console.log('File copied!');
});
```

### Synchronous:
```js
fs.copyFileSync('file.txt', 'copy.txt');
console.log('File copied!');
```

---

## 🔹 5. Check if File Exists

```js
if (fs.existsSync('file.txt')) {
  console.log('File exists!');
} else {
  console.log('File does not exist.');
}
```

---

## 🔹 6. Creating Directories

```js
fs.mkdir('newFolder', (err) => {
  if (err) throw err;
  console.log('Folder created!');
});
```

Recursive (nested):
```js
fs.mkdirSync('parent/child', { recursive: true });
console.log('Nested folder created!');
```

---

## 🔹 7. Deleting Directories

```js
fs.rmdir('newFolder', (err) => {
  if (err) throw err;
  console.log('Folder deleted!');
});
```

Recursive delete:
```js
fs.rmSync('parent', { recursive: true, force: true });
console.log('Folder and contents deleted!');
```

---

## ✅ Summary Table

| Operation     | Method(s)                   | Notes |
|---------------|-----------------------------|-------|
| Rename File   | `fs.rename`, `fs.renameSync` | Can also move files |
| Delete File   | `fs.unlink`, `fs.unlinkSync` | Removes files |
| Move File     | `fs.rename` with new path    | Moves file to another folder |
| Copy File     | `fs.copyFile`, `fs.copyFileSync` | Creates a duplicate |
| Check Exists  | `fs.existsSync`              | Returns true/false |
| Create Folder | `fs.mkdir`, `fs.mkdirSync`   | Use `{ recursive: true }` for nested |
| Delete Folder | `fs.rmdir`, `fs.rmSync`      | Use `{ recursive: true }` for force delete |

---

👉 These operations are **commonly asked in interviews** because they show understanding of Node.js `fs` module.  
