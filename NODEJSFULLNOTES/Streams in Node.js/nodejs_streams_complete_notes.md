# Streams in Node.js

## 🔹 Introduction

In Node.js, a **Stream** is a way to handle data that is **read or
written continuously**, instead of loading the entire data into memory
at once.

👉 Think of a stream like water flowing through a pipe:\
You don't wait for the entire tank to fill before you drink --- you
drink as it flows.

------------------------------------------------------------------------

## 🔹 Why Streams?

-   **Without streams**: Large files (e.g., 1GB) are fully loaded into
    memory → slow & memory heavy.\
-   **With streams**: Data is processed chunk by chunk → faster &
    efficient.

✅ Benefits: - Performance boost\
- Reduced memory usage

------------------------------------------------------------------------

## 🔹 Types of Streams

  ------------------------------------------------------------------------------------
  Stream Type           Direction          Example                    Use Case
  --------------------- ------------------ -------------------------- ----------------
  **Readable**          Source → Program   `fs.createReadStream()`    Reading data
                                                                      from files or
                                                                      HTTP requests

  **Writable**          Program →          `fs.createWriteStream()`   Writing logs,
                        Destination                                   files, or HTTP
                                                                      responses

  **Duplex**            Read + Write       `net.Socket`               Network
                                                                      communication
                                                                      (TCP)

  **Transform**         Read + Write +     `zlib.createGzip()`        Compression,
                        Transform                                     encryption, data
                                                                      conversion
  ------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 🔹 Buffer vs Stream

  -----------------------------------------------------------------------
  Feature                   Buffer                 Stream
  ------------------------- ---------------------- ----------------------
  **Data Handling**         Loads entire data at   Processes data in
                            once                   chunks

  **Memory Usage**          High (needs full data  Low (only handles
                            in memory)             chunks at a time)

  **Performance**           Slower for large files Faster & efficient for
                                                   large files

  **Use Case**              Small files, binary    Large files,
                            conversions, crypto    streaming, network
                                                   data
  -----------------------------------------------------------------------

✅ **Summary:**\
- **Buffer** = Store everything in memory first.\
- **Stream** = Process data piece by piece while it arrives.

------------------------------------------------------------------------

## 🔹 YouTube Streaming Example

  -----------------------------------------------------------------------
  Concept                       Explanation
  ----------------------------- -----------------------------------------
  **Chunks**                    YouTube video is split into 2--10 sec
                                segments

  **Streaming**                 Browser downloads chunks one by one,
                                starts playing early

  **Adaptive Quality**          Adjusts chunk quality based on internet
                                speed

  **Buffering**                 Browser keeps small chunks locally to
                                avoid playback lag
  -----------------------------------------------------------------------

✅ YouTube = Stream (continuous flow) + Buffer (temporary storage).

------------------------------------------------------------------------

## 🔹 Internal Buffer and highWaterMark

  --------------------------------------------------------------------------
  Concept             Description             Default / Example
  ------------------- ----------------------- ------------------------------
  **Internal Buffer** Temporary memory that   Managed by Node.js internally
                      stores chunks before    
                      being read              

  **highWaterMark**   Max size of buffer      Default: 64 KB
                      before pausing read     

  **read() Method**   Manually reads data     Used with `'readable'` event
                      from buffer (paused     
                      mode)                   
  --------------------------------------------------------------------------

### Example:

``` js
const fs = require('fs');
const readable = fs.createReadStream('file.txt', { highWaterMark: 16 * 1024 }); // 16KB

readable.on('data', (chunk) => {
  console.log('Chunk size:', chunk.length);
});
```

------------------------------------------------------------------------

## 🔹 Events in Readable Stream

  -----------------------------------------------------------------------
  Event          Description                Example Usage
  -------------- -------------------------- -----------------------------
  **data**       Emitted when a chunk is    Process streaming data
                 available (flowing mode)   

  **end**        No more data left to read  Close or finalize reading

  **error**      Error occurred during      Handle safely
                 reading                    

  **open**       File successfully opened   Log file descriptor

  **close**      Stream closed or destroyed Cleanup resources

  **readable**   Data available in buffer   Use `.read()` manually
                 (paused mode)              

  **pause**      Stream paused manually     Stop data flow temporarily

  **resume**     Stream resumed after pause Continue data flow

  **destroy /    Stream manually destroyed  Handle cancellations
  aborted**                                 
  -----------------------------------------------------------------------

### Example:

``` js
const fs = require('fs');
const stream = fs.createReadStream('input.txt', { encoding: 'utf8' });

stream.on('open', (fd) => console.log('File opened:', fd));
stream.on('data', (chunk) => console.log('Chunk:', chunk));
stream.on('pause', () => console.log('Paused.'));
stream.on('resume', () => console.log('Resumed.'));
stream.on('end', () => console.log('Finished reading.'));
stream.on('close', () => console.log('Closed.'));
stream.on('error', (err) => console.error('Error:', err));
```

------------------------------------------------------------------------

## 🔹 Flow Modes Summary

  --------------------------------------------------------------------------
  Mode        Trigger        How Data Flows          Controlled By
  ----------- -------------- ----------------------- -----------------------
  **Flowing   `'data'` event Automatically emits     Node.js
  Mode**                     chunks                  

  **Paused    `'readable'`   Manually pull chunks    Developer
  Mode**      event          using `.read()`         
  --------------------------------------------------------------------------

------------------------------------------------------------------------

## ✅ Overall Summary

-   Streams allow data to be processed **piece-by-piece**.\
-   Buffers temporarily hold **binary data**.\
-   **highWaterMark** controls internal buffer size.\
-   Readable streams have multiple **lifecycle events**.\
-   Used in **File I/O, HTTP, network, compression, media**.

------------------------------------------------------------------------
