# Node.js FS Module - Introduction

The **`fs` module** in Node.js allows you to interact with the file system. You can read, write, update, delete, and manage files and directories using this module. It provides both **synchronous** and **asynchronous** methods.

---

## 1. Importing the FS Module
```javascript
const fs = require('fs');
```

---

## 2. Key Features
- **Read files** – Read contents of a file.
- **Write files** – Create or overwrite files.
- **Append files** – Add content to existing files.
- **Delete files** – Remove files.
- **Rename files** – Rename existing files.
- **Check file stats** – Get file info like size, creation date, etc.
- **Work with directories** – Create, read, or remove directories.

---

## 3. Common FS Methods

### a) Reading Files
**Asynchronous:**
```javascript
fs.readFile('example.txt', 'utf8', (err, data) => {
    if(err) throw err;
    console.log(data);
});
```

**Synchronous:**
```javascript
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data);
```

### b) Writing Files
**Asynchronous:**
```javascript
fs.writeFile('example.txt', 'Hello World!', (err) => {
    if(err) throw err;
    console.log('File written successfully!');
});
```

**Synchronous:**
```javascript
fs.writeFileSync('example.txt', 'Hello World!');
console.log('File written successfully!');
```

### c) Appending to Files
```javascript
fs.appendFile('example.txt', '\nThis is appended text.', (err) => {
    if(err) throw err;
    console.log('Text appended!');
});
```

### d) Deleting Files
```javascript
fs.unlink('example.txt', (err) => {
    if(err) throw err;
    console.log('File deleted!');
});
```

### e) Renaming Files
```javascript
fs.rename('old.txt', 'new.txt', (err) => {
    if(err) throw err;
    console.log('File renamed!');
});
```

### f) Checking File Info
```javascript
fs.stat('example.txt', (err, stats) => {
    if(err) throw err;
    console.log(`File size: ${stats.size} bytes`);
    console.log(`Is file: ${stats.isFile()}`);
    console.log(`Created on: ${stats.birthtime}`);
});
```

### g) Working with Directories
```javascript
// Create a directory
fs.mkdir('myFolder', (err) => {
    if(err) throw err;
    console.log('Directory created!');
});

// Read directory
fs.readdir('.', (err, files) => {
    if(err) throw err;
    console.log(files);
});

// Remove directory
fs.rmdir('myFolder', (err) => {
    if(err) throw err;
    console.log('Directory removed!');
});
```

---

## 4. Synchronous vs Asynchronous Methods
| Feature | Asynchronous | Synchronous |
|---------|--------------|-------------|
| Execution | Non-blocking | Blocking |
| Use Case | Server apps | Simple scripts |
| Callback | Required | Not required |

---

## 5. Summary
- `fs` is a core Node.js module for handling files and directories.
- Provides both **sync** and **async** methods.
- Essential for reading, writing, appending, deleting, and managing files in Node.js.

```js

🔹 1. fs.stat (File Information)
What it does:

Provides metadata (information) about a file or directory.

Info includes size, creation time, modification time, file type, etc.

Example:
const fs = require('fs');

fs.stat('demo.txt', (err, stats) => {
  if (err) throw err;
  console.log(stats);
  console.log("Is file? ", stats.isFile());
  console.log("Is directory? ", stats.isDirectory());
  console.log("File size: ", stats.size, "bytes");
});

Sample Output:
Stats {
  dev: 123456,
  mode: 33188,
  size: 56,
  atime: 2025-08-18T07:30:00Z,
  mtime: 2025-08-18T07:31:00Z,
  ctime: 2025-08-18T07:31:00Z,
  birthtime: 2025-08-18T07:29:00Z
}
Is file?  true
Is directory?  false
File size:  56 bytes


👉 Use fs.stat when you need details about a file (size, timestamps, type, etc.).

🔹 2. fs.watch (File Watcher)
What it does:

Watches a file or directory for changes.

Triggers a callback whenever the file changes (modified, renamed, deleted, etc.).

Example:
const fs = require('fs');

fs.watch('demo.txt', (eventType, filename) => {
  console.log(`File ${filename} changed! Event: ${eventType}`);
});

Sample Output:
File demo.txt changed! Event: change
File demo.txt changed! Event: rename

Key Points:

eventType → "rename" or "change".

filename → the file that changed (sometimes may be null on some OS).

Useful for real-time monitoring, like log files, config changes, auto-reload, etc.

✅ Difference Between fs.stat vs fs.watch
Feature	fs.stat 📝 (Info)	fs.watch 👀 (Monitor)
Purpose	Get metadata of file/folder	Watch file/folder for changes
Returns	File details (stats object)	Events (change, rename)
Usage	Size, type, timestamps	Auto-reload, live monitoring
Execution	One-time call	Continuous event listener

👉 In short:

Use stat if you want to know about a file.

Use watch if you want to react to changes in a file.