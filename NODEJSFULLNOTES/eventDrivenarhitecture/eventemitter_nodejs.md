
# EventEmitter in Node.js & Custom EventEmitter

## 1. What is EventEmitter?
- In Node.js, the **`events` module** provides the **EventEmitter** class.  
- It allows us to **emit (produce)** events and **listen (consume)** to them.  
- This is the **core of Node.js’ event-driven architecture**.  

---

## 2. Basic Example
```js
const EventEmitter = require('events');

// Create an emitter
const emitter = new EventEmitter();

// Add a listener
emitter.on('greet', (name) => {
  console.log(`Hello, ${name}!`);
});

// Emit an event
emitter.emit('greet', 'Himanshu');
```
**Output:**
```
Hello, Himanshu!
```

---

## 3. Common Methods
- `.on(event, listener)` → Register a listener.  
- `.emit(event, ...args)` → Trigger the event with arguments.  
- `.once(event, listener)` → Run listener **only the first time**.  
- `.off(event, listener)` or `.removeListener()` → Remove a specific listener.  
- `.removeAllListeners(event)` → Remove all listeners for an event.  
- `.listenerCount(event)` → Count how many listeners are attached.  

---

## 4. Example with Multiple Listeners
```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('dataReceived', () => {
  console.log("Listener 1: Data received!");
});

emitter.on('dataReceived', () => {
  console.log("Listener 2: Logging data...");
});

emitter.emit('dataReceived');
```
**Output:**
```
Listener 1: Data received!
Listener 2: Logging data...
```

---

# Custom EventEmitter

## 1. Why Custom EventEmitter?
- Sometimes we want our own **classes** to behave like EventEmitters.  
- We can extend the built-in **EventEmitter class**.  

---

## 2. Example: Custom EventEmitter
```js
const EventEmitter = require('events');

class OrderSystem extends EventEmitter {
  placeOrder(orderId) {
    console.log(`Order ${orderId} placed.`);
    this.emit('orderPlaced', orderId);
  }
}

const order = new OrderSystem();

// Listener for custom event
order.on('orderPlaced', (id) => {
  console.log(`Processing order: ${id}`);
});

order.placeOrder(101);
```
**Output:**
```
Order 101 placed.
Processing order: 101
```

---

## 3. Real-Life Example
- **Producer:** Payment Service → emits `"paymentSuccess"` event.  
- **Consumers:**  
  - Notification Service → sends confirmation email.  
  - Analytics Service → logs transaction.  
  - Inventory Service → updates stock.  

This way, services are **loosely coupled**.

---

## 4. Mermaid Diagram – Event Flow
```mermaid
flowchart LR
    A[Custom Class / Producer] -->|emit(event)| B[EventEmitter]
    B --> C1[Listener 1]
    B --> C2[Listener 2]
    B --> C3[Listener 3]
```

---

## 5. Summary
- **EventEmitter** = Core class in Node.js for event-driven communication.  
- Used for **listening & emitting events**.  
- **Custom EventEmitter** = Extend `EventEmitter` in your own classes for domain-specific events.  
- Helps achieve **modular, decoupled, scalable architecture**.  
