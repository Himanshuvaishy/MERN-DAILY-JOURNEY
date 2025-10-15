# Writable Streams in Node.js

## 🔹 Introduction

A **Writable Stream** is used to **write data** to a destination
sequentially (piece by piece), such as writing to a file, sending a
network response, or compressing data.

👉 Writable streams are the **opposite of readable streams** --- they
consume data instead of producing it.

------------------------------------------------------------------------

## 🔹 Common Examples of Writable Streams

  Example                 Stream
  ----------------------- ------------------------------
  Writing to a file       `fs.createWriteStream()`
  Sending HTTP response   `res` object in HTTP/Express
  Writing to a socket     `net.Socket`
  Compression output      `zlib.createGzip()`

------------------------------------------------------------------------

## 🔹 Example --- Writing to a File

``` js
const fs = require('fs');
const writable = fs.createWriteStream('output.txt');

writable.write('Hello ');
writable.write('World!');
writable.end(' ✅ Stream closed.');

console.log('Data writing started...');
```

🧠 **Explanation:** - `write()` adds data to the internal buffer.\
- `end()` signals that no more data will be written.\
- The file will contain: `Hello World! ✅ Stream closed.`

------------------------------------------------------------------------

## 🔹 Internal Buffer of Writable Streams

Each writable stream maintains an **internal buffer** that temporarily
stores data before writing it to the destination.

  --------------------------------------------------------------------------
  Concept             Description             Default / Example
  ------------------- ----------------------- ------------------------------
  **Internal Buffer** Temporary storage       Managed internally by Node.js
                      before writing          

  **highWaterMark**   Maximum buffer size     Default: 16 KB (for strings)
                      before pausing          

  **write()**         Adds data to buffer     Returns `false` if buffer is
                                              full

  **drain Event**     Triggered when buffer   Resume writing safely
                      is empty again          
  --------------------------------------------------------------------------

### 🧩 Example (Internal Buffer & `drain`)

``` js
const fs = require('fs');
const stream = fs.createWriteStream('bigfile.txt', { highWaterMark: 16 * 1024 }); // 16 KB buffer

for (let i = 0; i < 1e5; i++) {
  const canWrite = stream.write(`Line ${i}\n`);
  if (!canWrite) {
    console.log('⏸ Buffer full — waiting for drain...');
    stream.once('drain', () => console.log('🚀 Buffer drained, continue writing.'));
  }
}
stream.end('✅ Finished writing.');
```

🧠 **Explanation:** - Data is written until the internal buffer fills
up.\
- When full, `write()` returns `false`.\
- `'drain'` fires once the buffer is empty → writing can resume.

------------------------------------------------------------------------

## 🔹 Backpressure

**Backpressure** occurs when the **destination cannot handle data** as
quickly as the source is sending it.

🧠 **Analogy:**\
Pouring water into a glass --- if you pour too fast, it overflows.\
That overflow is **backpressure** 💧

------------------------------------------------------------------------

### 🧩 Example (Handling Backpressure)

``` js
const fs = require('fs');

const readable = fs.createReadStream('input.txt');
const writable = fs.createWriteStream('output.txt');

readable.on('data', (chunk) => {
  const ok = writable.write(chunk);
  if (!ok) {
    console.log('⏸ Buffer full — pausing reading...');
    readable.pause();
  }
});

writable.on('drain', () => {
  console.log('🚀 Buffer drained — resuming reading...');
  readable.resume();
});

readable.on('end', () => {
  writable.end();
  console.log('✅ Data transfer complete.');
});
```

✅ **Explanation:** - `write()` returns `false` when buffer is full →
**pause** the readable stream.\
- `'drain'` event signals it's safe to **resume** reading.\
- Prevents data loss and ensures smooth transfer.

------------------------------------------------------------------------

## 🔹 Closing Writable Streams

When done writing, you must **close the stream** properly to ensure all
data is flushed to disk.

  Method / Event   Purpose
  ---------------- ----------------------------------------
  **`end()`**      Close stream after final write
  **`finish`**     All data flushed successfully
  **`close`**      Resource released (file handle closed)
  **`error`**      Error occurred while writing

### 🧩 Example (Proper Closing)

``` js
const fs = require('fs');
const writable = fs.createWriteStream('output.txt');

writable.write('This is first line.\n');
writable.write('This is second line.\n');
writable.end('✅ Stream closed.\n');

writable.on('finish', () => console.log('All data flushed!'));
writable.on('close', () => console.log('Stream fully closed.'));
writable.on('error', (err) => console.error('Error:', err));
```

🧠 **Explanation:** - `finish` → emitted when all buffered data is
written.\
- `close` → emitted after resource cleanup.

------------------------------------------------------------------------

## 🔹 States of Writable Streams

  State            Description
  ---------------- ---------------------------------------------------
  **Writable**     Stream ready to accept new data (`write()` works)
  **Corked**       Data temporarily held (via `.cork()`)
  **Need Drain**   Buffer full, waiting for `'drain'` event
  **Finished**     `end()` called, all data flushed
  **Closed**       Stream resource fully released
  **Destroyed**    Stream manually destroyed or errored

### 🧩 Example (Writable Stream States)

``` js
const fs = require('fs');
const writable = fs.createWriteStream('demo.txt');

console.log('Initially writable?', writable.writable); // true

writable.write('Hello Node Streams!\n');
writable.cork(); // Hold data temporarily
writable.write('Buffered line while corked\n');
writable.uncork(); // Flush data

writable.end('✅ End of file');
```

🧠 **Explanation:** - `.cork()` holds writes temporarily to optimize
I/O.\
- `.uncork()` flushes them to the file.\
- `.end()` closes the stream.

------------------------------------------------------------------------

## ✅ Summary

  Concept                      Description
  ---------------------------- ------------------------------------------------
  **Writable Stream**          Used to write data (file, HTTP, network, etc.)
  **Internal Buffer**          Temporarily stores data before writing
  **highWaterMark**            Controls internal buffer size
  **Backpressure**             Occurs when destination is slower than source
  **drain Event**              Signals buffer is empty again
  **end() / finish / close**   Used for graceful shutdown
  **States**                   Writable → Need Drain → Finished → Closed

------------------------------------------------------------------------
