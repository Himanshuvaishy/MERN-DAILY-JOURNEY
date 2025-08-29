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

