# Creating HTTP Responses Using the `net` Module (Raw TCP) — Notes

When you build an HTTP server using Node.js's **`net`** module (raw TCP sockets) instead of the `http` module, you must **manually write the HTTP status line and headers** to the socket before sending the body. Browsers expect headers separated from the body by a blank line (two CRLFs).

> **Important**: HTTP requires CRLF (`\r\n`) as line endings. Use `\r\n` instead of `\n` to avoid subtle bugs with some clients.

---

## ✅ Minimal correct response structure
```
HTTP/1.1 200 OK\r\n
Header-Name: header-value\r\n
Another-Header: value\r\n
\r\n
[binary body bytes here]
```

---

## Example — Serving a static file (correct headers & Content-Length)
```js
// net-http-static.js
const net = require('net');
const fs = require('fs/promises');
// or: const fs = require('fs');

const server = net.createServer(async (socket) => {
  try {
    const fh = await fs.open('numbers.txt'); // Node's fs/promises FileHandle
    const { size } = await fh.stat();

    // Write status line and headers (CRLF required)
    socket.write('HTTP/1.1 200 OK\r\n');
    socket.write('Content-Type: text/plain; charset=utf-8\r\n');
    socket.write(`Content-Length: ${size}\r\n`);
    socket.write('Connection: close\r\n');
    socket.write('\r\n');

    // Pipe file stream to socket (this will end socket by default once done)
    const stream = fh.createReadStream();
    stream.pipe(socket);

    stream.on('end', () => fh.close());
    stream.on('error', (err) => {
      console.error('Stream error:', err);
      socket.destroy();
      fh.close();
    });
  } catch (err) {
    socket.write('HTTP/1.1 500 Internal Server Error\r\n\r\n');
    socket.end();
  }
});

server.listen(8080, () => console.log('Raw-TCP HTTP server listening on port 8080'));
```

**Notes:**
- `Content-Length` tells the client exactly how many bytes to read for the body. The browser will stop reading once it has read that many bytes.
- Add `Connection: close` to indicate the server will close the connection once the response is delivered (helpful for simple servers).

---

## ⚙️ Important Response Headers (explanations)

### 1) `Content-Length`
- **What it does:** Tells the client the exact number of bytes in the response body.
- **Why important:** If omitted, the client may wait or rely on the server closing the connection to detect end-of-body. For persistent connections, `Content-Length` (or `Transfer-Encoding`) is required.
- **Example:** `Content-Length: 1024`

### 2) `Content-Type`
- **What it does:** Tells the client how to interpret the body (MIME type).
- **Examples:**
  - `Content-Type: text/html; charset=utf-8`
  - `Content-Type: text/plain; charset=utf-8`
  - `Content-Type: application/json`
  - `Content-Type: image/png`

### 3) `Content-Disposition`
- **What it does:** Suggests how content should be handled — inline or attachment (download).
- **Examples:**
  - Inline view: `Content-Disposition: inline`
  - Download with filename: `Content-Disposition: attachment; filename="hello.pdf"`
- Use semicolons to add parameters (filename, size, etc.).

### 4) `Transfer-Encoding`
- **What it does:** Used when you cannot (or do not want to) set `Content-Length`.
- **Example:** `Transfer-Encoding: chunked`
- **Chunked encoding:** the body is sent as a series of chunks, each with its size in hex followed by CRLF, the chunk data, and a trailing CRLF. End with a `0\r\n\r\n` chunk.
- **When to use:** Dynamic responses (generated on the fly), streaming where total size unknown.

### 5) `Connection`
- **What it does:** Controls whether the connection should be kept alive.
- **Examples:** `Connection: keep-alive` or `Connection: close`
- For raw TCP servers that don't implement persistent connections well, `Connection: close` is safe.

### 6) `Content-Encoding`
- **What it does:** Indicates body compression (gzip, br, deflate).
- Example: `Content-Encoding: gzip`
- If you compress the body, set `Content-Encoding` and update `Content-Length` to compressed size (or use chunked).

### 7) `Cache-Control`, `Date`, `ETag`, `Last-Modified`, `Server`
- `Cache-Control`: caching rules (`no-cache`, `max-age=3600`, etc.)
- `Date`: response timestamp (e.g., `Date: Wed, 07 Nov 2025 12:00:00 GMT`)
- `ETag` / `Last-Modified`: validators for conditional requests
- `Server`: identify server software (optional)

---

## Example — Chunked Transfer-Encoding (dynamic streaming)
```js
// net-http-chunked.js
const net = require('net');

const server = net.createServer((socket) => {
  socket.write('HTTP/1.1 200 OK\r\n');
  socket.write('Content-Type: text/plain; charset=utf-8\r\n');
  socket.write('Transfer-Encoding: chunked\r\n');
  socket.write('Connection: close\r\n');
  socket.write('\r\n');

  // Send three chunks with delays (size in hex + CRLF, data, CRLF)
  function sendChunk(data) {
    const buf = Buffer.from(String(data));
    socket.write(buf.length.toString(16) + '\r\n');
    socket.write(buf);
    socket.write('\r\n');
  }

  sendChunk('Hello, chunk 1\n');
  setTimeout(() => sendChunk('Chunk 2: more data\n'), 500);
  setTimeout(() => {
    sendChunk('Last chunk\n');
    // terminating chunk
    socket.write('0\r\n\r\n');
    socket.end();
  }, 1000);
});

server.listen(8081, () => console.log('Chunked server on port 8081'));
```

**Note:** Browsers understand chunked encoding and will render as data arrives.

---

## ⚠️ Common Pitfalls & Tips
- **Use CRLF (`\r\n`) line endings** — many clients are tolerant, but strict clients require CRLF.
- **Always include a blank line (`\r\n\r\n`) between headers and body.**
- **If you set `Content-Length`, ensure the bytes sent exactly match it.** Otherwise the client may hang waiting or cut off early.
- **When using `readStream.pipe(socket)`**, the stream will end the socket by default. If you need the connection kept alive, avoid ending the socket and manage bytes carefully.
- **For partial content (range requests)** implement `206 Partial Content` and `Content-Range` headers.
- **HTTPS over raw sockets requires TLS/TLS wrapping** — use `tls` or `https` module for secure servers.

---

## ✅ Quick checklist when writing HTTP over `net`
- Use correct CRLF line endings.
- Send a proper status line (`HTTP/1.1 200 OK`).
- Include `Content-Type`.
- Either include `Content-Length` **or** `Transfer-Encoding: chunked`.
- Close the connection or implement keep-alive correctly.
- Handle errors to avoid leaking file handles and sockets.

---

**Created by:** GPT-5  
**Topic:** Creating HTTP Server using `net` Module & Important Response Headers  
**Date:** 2025
