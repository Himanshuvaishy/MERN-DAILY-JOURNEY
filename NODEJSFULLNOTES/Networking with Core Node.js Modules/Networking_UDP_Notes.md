# Networking in Node.js — UDP and Sockets (Brief Notes)

## 🧠 What is a Socket?
A **socket** is one endpoint of a two-way communication channel between computers.
It allows sending and receiving data through a network.

There are two main types:
- **TCP (Stream Sockets):** Reliable, ordered, connection-based communication.
- **UDP (Datagram Sockets):** Fast, connectionless, message-based communication (no guarantee of delivery).

---

## 📦 The `dgram` Module (UDP in Node.js)
Node.js provides the **`dgram`** module to work with UDP sockets.

### Import
```js
const dgram = require('dgram');
```

---

## 🖥 Creating a UDP Server
```js
const dgram = require('dgram');
const server = dgram.createSocket('udp4');

server.on('message', (msg, rinfo) => {
  console.log(`Received ${msg} from ${rinfo.address}:${rinfo.port}`);
  server.send(msg, rinfo.port, rinfo.address);
});

server.bind(41234, '0.0.0.0');
```
**Explanation:**
- `createSocket('udp4')`: Creates a UDP socket using IPv4.
- `server.on('message')`: Handles incoming messages.
- `server.send()`: Sends data back to a client.
- `server.bind()`: Starts listening on a port.

---

## 💻 Creating a UDP Client
```js
const dgram = require('dgram');
const client = dgram.createSocket('udp4');

const message = Buffer.from('Hello Server!');
client.send(message, 41234, 'localhost');

client.on('message', (msg) => {
  console.log('Server replied:', msg.toString());
  client.close();
});
```
**Explanation:**
- `client.send()`: Sends a UDP message to the server.
- `client.on('message')`: Listens for a reply from the server.

---

## ⚙️ Common UDP Methods
| Method | Description |
|--------|--------------|
| `socket.bind(port)` | Bind socket to a port to receive messages. |
| `socket.send(msg, port, address)` | Send a UDP datagram. |
| `socket.close()` | Close the socket. |
| `socket.setBroadcast(true)` | Enable sending to broadcast address. |
| `socket.addMembership(multicastAddress)` | Join a multicast group. |

---

## 📡 Broadcast Example
```js
server.bind(() => {
  server.setBroadcast(true);
  const msg = Buffer.from('Broadcast message');
  server.send(msg, 41234, '255.255.255.255');
});
```

---

## ⚠️ UDP Key Points
- No connection — every packet is independent.
- Not reliable (packets may be lost).
- Low latency and fast.
- Good for real-time apps (games, video, voice).

---

## 🧩 Summary
| Concept | Description |
|----------|--------------|
| **Socket** | Communication endpoint. |
| **UDP (Datagram)** | Fast, connectionless, unreliable messages. |
| **`dgram` module** | Node.js module for UDP communication. |
| **Server** | Listens for incoming UDP datagrams. |
| **Client** | Sends datagrams to the server. |

---

**✅ Use Case Examples:**
- Online games
- Video or audio streaming
- IoT sensor data
- Local service discovery

---

**Created by:** GPT-5  
**Topic:** Networking with Core Node.js Modules — UDP & Sockets
