# Piping Streams in Node.js

## 🔹 Introduction

When working with streams in Node.js, we often need to transfer data
from a **Readable Stream** to a **Writable Stream**.\
This can be done manually or using the built-in **`.pipe()`** method,
which automatically handles **backpressure**.

------------------------------------------------------------------------

## 🧩 Normal / Manual Method

So far, this is how we have been writing data manually from a readable
stream to a writable stream.

``` js
import fs from "node:fs";

let readStream = fs.createReadStream(
  "C://Users//shail//Downloads//Telegram Desktop//Harkirat Course 0 t o100//[Ad-hoc] 1. Next.js.mp4",
  { highWaterMark: 1 * 1024 * 1024 }
);

let writeStream = fs.createWriteStream("video-mp4");

readStream.on("data", (chunk) => {
  let write = writeStream.write(chunk);
  if (!write) {
    readStream.pause();
  }
});

writeStream.on("drain", () => {
  readStream.resume();
});

readStream.on("end", () => {
  writeStream.end();
});

writeStream.on("finish", () => console.log("Finished"));
writeStream.on("close", () => console.log("Closed"));
```

### ⚙️ Explanation:

-   Data is manually read in chunks using `'data'` event.
-   If the buffer is full → stream is paused.
-   `'drain'` resumes reading when writable stream is ready again.
-   `'end'` signals end of data transfer.

------------------------------------------------------------------------

## 🔹 Pipe Method

Using the **`.pipe()`** method makes this process simpler and
automatically handles **backpressure** internally.

``` js
import fs from "node:fs";

let readStream = fs.createReadStream(
  "C://Users//shail//Downloads//Telegram Desktop//Harkirat Course 0 t o100//[Ad-hoc] 1. Next.js.mp4",
  { highWaterMark: 1 * 1024 * 1024 }
);

let writeStream = fs.createWriteStream("video-mp4");

readStream.pipe(writeStream); // ✅ Pipe method

console.log("Piping started...");

writeStream.on("finish", () => console.log("Finished"));
writeStream.on("close", () => console.log("Closed"));
```

### 🧠 Key Points:

-   `.pipe()` connects the readable and writable streams.
-   It **automatically pauses/resumes** streams to handle
    **backpressure**.
-   Ideal for copying files, streaming videos, or sending HTTP
    responses.

------------------------------------------------------------------------

## 🔹 Unpipe Method

To **stop data flow** between readable and writable streams, use
`.unpipe()`.

``` js
import fs from "node:fs";

let readStream = fs.createReadStream(
  "C://Users//shail//Downloads//Telegram Desktop//Harkirat Course 0 t o100//[Ad-hoc] 1. Next.js.mp4",
  { highWaterMark: 1 * 1024 * 1024 }
);

let writeStream = fs.createWriteStream("video-mp4", {
  highWaterMark: 1 * 1024 * 1024,
});

readStream.pipe(writeStream);

console.log("Piping Started...");

setTimeout(() => {
  console.log(readStream.unpipe(writeStream)); // 🔹 Unpipe method
}, 1000);

writeStream.on("finish", () => console.log("Finished"));
writeStream.on("close", () => console.log("Closed"));
```

### ⚙️ Explanation:

-   Stops transferring data between streams.
-   Only partial data is written before stopping.
-   Used for conditional transfers or manual interruption.

------------------------------------------------------------------------

## 🔹 Limitation of Pipe

> Using `.pipe()` does **not handle errors** effectively --- if an error
> occurs, you must manually add listeners.

------------------------------------------------------------------------

## 🔹 Piping using Pipeline (Error Handling)

The **`pipeline()`** function (introduced in Node.js v10) improves upon
`.pipe()` by providing **automatic error handling**.

``` js
import fs from "node:fs";
import { pipeline } from "node:stream";

let readStream = fs.createReadStream(
  "C://Users//shail//Downloads//Telegram Desktop//Harkirat Course 0 t o100//[Ad-hoc] 1. Next.js.mp4",
  { highWaterMark: 1 * 1024 * 1024 }
);

let writeStream = fs.createWriteStream("video-mp4", {
  highWaterMark: 1 * 1024 * 1024,
});

pipeline(readStream, writeStream, (err) => {
  console.log(err); // Handles error automatically
});

setTimeout(() => {
  readStream.destroy("Error");
}, 1000);
```

### ✅ Advantages of `pipeline()`:

  Feature                        Description
  ------------------------------ ------------------------------------------
  **Automatic Error Handling**   Catches errors from any stream
  **Clean Syntax**               No need for manual pause/resume
  **Recommended**                Official Node.js standard over `.pipe()`

------------------------------------------------------------------------

## 🔹 Example with Continuous Process

``` js
import fs from "node:fs";
import { pipeline } from "node:stream";

let readStream = fs.createReadStream(
  "C://Users//shail//Downloads//Telegram Desktop//Harkirat Course 0 t o100//[Ad-hoc] 1. Next.js.mp4",
  { highWaterMark: 1 * 1024 * 1024 }
);

let writeStream = fs.createWriteStream("video-mp4", {
  highWaterMark: 1 * 1024 * 1024,
});

pipeline(readStream, writeStream, (err) => {
  console.log(err);
});

setTimeout(() => {
  readStream.destroy("Error");
}, 1000);

setInterval(() => {
  console.log("hy");
}, 1000);
```

------------------------------------------------------------------------

## 🔹 Summary Table

  -----------------------------------------------------------------------
  Concept                       Description
  ----------------------------- -----------------------------------------
  **Manual Method**             Manually read & write with `'data'`,
                                `'drain'`, `'end'`

  **Pipe Method**               Automatically handles backpressure

  **Unpipe Method**             Stops data flow between streams

  **Pipeline()**                Handles errors automatically and safely
                                closes streams
  -----------------------------------------------------------------------

------------------------------------------------------------------------
