# Advanced Streams in Node.js

---

## 1. Duplex Streams

**Definition:**  
A **Duplex Stream** is a type of stream that is **both readable and writable**.  
This means you can both read data from it and write data into it.

### Example:
```js
import { Duplex } from "node:stream";

const duplex = new Duplex({
  read(size) {
    this.push("Hello from Duplex Stream");
    this.push(null); // signals end of reading
  },
  write(chunk, encoding, callback) {
    console.log("Writing:", chunk.toString());
    callback();
  },
});

duplex.on("data", (chunk) => console.log("Received:", chunk.toString()));
duplex.write("Hi Duplex!");
duplex.end();
```
**Output:**
```
Writing: Hi Duplex!
Received: Hello from Duplex Stream
```

---

## 2. Transform Streams

**Definition:**  
A **Transform Stream** is a special type of Duplex stream that can **modify or transform** the data while reading or writing.

It is commonly used for compression, encryption, or data formatting.

### Example:
```js
import { Transform } from "node:stream";

const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  },
});

process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```
**Explanation:**  
- Whatever you type in the terminal will be converted to **uppercase** and printed.

---

## 3. PassThrough Streams

**Definition:**  
A **PassThrough Stream** is a special type of Transform stream that **does not modify the data** — it simply passes it through.

Useful for debugging or monitoring data flow.

### Example:
```js
import { PassThrough } from "node:stream";
import fs from "node:fs";

const pass = new PassThrough();

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(pass).pipe(writeStream);

pass.on("data", (chunk) => {
  console.log("Passing through:", chunk.toString());
});
```
**Explanation:**  
The `PassThrough` stream allows you to **inspect** the data as it flows without altering it.

---

## 4. Data Streams

**Definition:**  
**Data Streams** refer to **continuous flow of data** between sources and destinations — like files, network sockets, APIs, etc.

| Type | Description | Example |
|------|--------------|----------|
| Readable | Used for reading data | `fs.createReadStream()` |
| Writable | Used for writing data | `fs.createWriteStream()` |
| Duplex | Read and write | `net.Socket` |
| Transform | Modify data | `zlib.createGzip()` |

---

## 5. Piping Streams

**Definition:**  
**Piping** connects the **output of one stream** to the **input of another stream**.

This avoids manually managing events and backpressure.

### Example:
```js
import fs from "node:fs";

const readStream = fs.createReadStream("input.txt");
const writeStream = fs.createWriteStream("output.txt");

readStream.pipe(writeStream);
```
**Explanation:**  
The `pipe()` method automatically handles **flow control** (backpressure).

---

## 6. Redirection of Data Streams

**Definition:**  
In Node.js, you can redirect data between streams using **pipe()** or **pipeline()**.  
It’s similar to shell redirection (e.g., `cat file.txt > output.txt`).

### Example:
```js
import fs from "node:fs";
import { pipeline } from "node:stream";

pipeline(
  fs.createReadStream("source.txt"),
  fs.createWriteStream("destination.txt"),
  (err) => {
    if (err) console.error("Pipeline failed:", err);
    else console.log("Pipeline succeeded!");
  }
);
```

**Explanation:**  
`pipeline()` provides **error handling** automatically, unlike simple `pipe()`.

---

## Summary Table

| Stream Type | Readable | Writable | Transforms Data | Example |
|--------------|-----------|-----------|-----------------|----------|
| Readable | ✅ | ❌ | ❌ | `fs.createReadStream()` |
| Writable | ❌ | ✅ | ❌ | `fs.createWriteStream()` |
| Duplex | ✅ | ✅ | ❌ | `net.Socket` |
| Transform | ✅ | ✅ | ✅ | `zlib.createGzip()` |
| PassThrough | ✅ | ✅ | ❌ (passes data) | `new PassThrough()` |

---

**✅ Key Takeaways:**
- **Duplex:** Read + Write  
- **Transform:** Read + Write + Modify  
- **PassThrough:** Read + Write + No modification  
- **Pipe:** Easy way to connect streams  
- **Pipeline:** Adds error handling over pipe()

---
