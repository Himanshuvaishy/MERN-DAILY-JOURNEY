# Streams in Node.js

## 🔹 Introduction

In Node.js, a **Stream** is a way to handle data that is **read or
written continuously**, instead of loading the entire data into memory
at once.

👉 Think of a stream like water flowing through a pipe:\
- You don't wait for the entire tank to fill before you drink.\
- Instead, you drink as it flows.

------------------------------------------------------------------------

## 🔹 Why Streams?

-   **Without streams**: Large files (e.g., 1GB) are fully loaded into
    memory → slow & memory heavy.\
-   **With streams**: Data is processed chunk by chunk → faster &
    efficient.

✅ Benefits: Performance boost & reduced memory usage.

------------------------------------------------------------------------

## 🔹 Types of Streams

1.  **Readable Streams** → Read data from source.\
    Example: `fs.createReadStream()`\
2.  **Writable Streams** → Write data to destination.\
    Example: `fs.createWriteStream()`\
3.  **Duplex Streams** → Both read & write.\
    Example: `net.Socket`\
4.  **Transform Streams** → Duplex streams that can modify data while
    passing.\
    Example: `zlib.createGzip()`

------------------------------------------------------------------------

## 🔹 Events in Streams

Streams are `EventEmitter` objects with common events:

-   `data` → when a chunk is available\
-   `end` → when no more data is left\
-   `error` → on errors\
-   `finish` → when writing is done

------------------------------------------------------------------------

## 🔹 Example: Reading a File

``` js
const fs = require('fs');

const readable = fs.createReadStream('bigfile.txt', { encoding: 'utf8' });

readable.on('data', (chunk) => {
  console.log('Chunk received:', chunk);
});

readable.on('end', () => {
  console.log('Finished reading file.');
});

readable.on('error', (err) => {
  console.error('Error:', err);
});
```

------------------------------------------------------------------------

## 🔹 Example: Copy File Using Pipe

``` js
const fs = require('fs');

const readable = fs.createReadStream('input.txt');
const writable = fs.createWriteStream('output.txt');

readable.pipe(writable);

console.log("File copied successfully!");
```

------------------------------------------------------------------------

## 🔹 Real-World Use Cases

-   Handling large files (logs, videos, etc.)\
-   Video/audio streaming (Netflix, YouTube)\
-   Network communication (TCP, HTTP)\
-   Data transformation (compression, encryption)

------------------------------------------------------------------------
# Buffer vs Stream in Node.js

## 🔹 Buffer
- A **buffer** is a temporary memory that holds binary data.
- In Node.js, buffers are mainly used for handling **binary data** (like files, TCP packets).
- Stores the **entire data** before processing.

### Example
```js
const fs = require('fs');

fs.readFile('bigfile.txt', (err, data) => {
  if (err) throw err;
  console.log("File size:", data.length);
});
⚠️ Problem → Loads whole file in memory → slow and memory heavy for large files.

🔹 Stream
A stream processes data chunk by chunk as it arrives.

No need to load everything into memory at once.

Efficient for large files, audio/video, and real-time data.

Example
js
Copy code
const fs = require('fs');

const readable = fs.createReadStream('bigfile.txt');

readable.on('data', (chunk) => {
  console.log("Received chunk:", chunk.length);
});
✅ Only small chunks are stored in memory → low memory usage.

## 🔹 Major Differences

| Feature         | Buffer                                | Stream                                |
|-----------------|---------------------------------------|---------------------------------------|
| **Data Handling** | Loads entire data at once             | Processes data in chunks              |
| **Memory Usage**  | High (needs full data in memory)      | Low (only handles chunks at a time)   |
| **Performance**   | Slower for large files                | Faster & efficient for large files    |
| **Use Case**      | Small files, binary conversions, crypto | Large files, streaming, network data  |


🔹 Why We Use Them
Buffers

For direct binary manipulation (images, encryption, TCP packets).

Suitable for small files where memory isn’t a big concern.

Streams

For large file handling (logs, backups, videos).

For real-time applications (Netflix, YouTube, live chat, HTTP).

✅ Summary:

Buffer = Store everything in memory first.

Stream = Process data piece by piece while it arrives.

------------------------------------------------------------------------
🔹 Do Both Buffer and Stream Transfer Data?

✅ Yes, both are involved in transferring data from one place to another, but the way they handle it is different.

🔹 Buffer

A buffer is like a storage box.

Data is first collected in memory (buffered completely) and then transferred/processed.

Suitable for small data transfers.

👉 Example:

fs.readFile() → loads the whole file into a buffer, then sends it.

Good for sending small images, configs, JSON files.

🔹 Stream

A stream is like a pipeline.

Data is transferred chunk by chunk while still being read/written.

Best for large or continuous transfers.

👉 Example:

fs.createReadStream() → starts sending data while still reading the file.

Good for video streaming, large backups, live chat, HTTP responses.

🔹 Key Idea

Buffer → "Store everything first, then transfer."

Stream → "Transfer as it flows, no need to wait for all data."

✅ So yes, both are used for data transfer, but the approach differs:

Buffer = batch transfer (all at once).

Stream = continuous transfer (piece by piece)..