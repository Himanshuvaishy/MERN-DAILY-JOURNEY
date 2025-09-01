# ⚡ Binary Data at the Physical Level

## 🔹 1. How Data is Stored at the Physical Level

Computers only understand **binary (0s and 1s)**, which represent two states:
- `0` → OFF (no electricity / low voltage / no magnetic charge)
- `1` → ON (electricity present / high voltage / magnetic charge)

### Storage Mediums:
1. **Hard Disk (HDD)**
   - Uses **magnetism**.
   - Tiny regions on the disk are magnetized in one direction (1) or the opposite direction (0).

2. **Solid-State Drive (SSD)**
   - Uses **transistors** with electric charge.
   - Charged state = 1, No charge = 0.

3. **RAM (Memory)**
   - Uses capacitors and transistors.
   - A charged capacitor = 1, discharged = 0.
   - Volatile → data disappears when power is off.

4. **Processor (CPU Registers, Cache)**
   - Uses tiny transistors (billions on a chip).
   - Open = 0, Closed = 1.
   - Operates at GHz speeds.

---

## 🔹 2. How Data is Transferred at the Physical Level

- **Inside CPU / RAM** → electrical signals travel through circuits as voltage pulses.
- **Between computers (network)** → signals are transferred as:
  - Electrical pulses (Ethernet cable, copper wires).
  - Light pulses (Fiber optic cable → 1 = light on, 0 = light off).
  - Radio waves (Wi-Fi, Bluetooth → modulated signals represent bits).

👉 At the lowest level, all communication = stream of **0s and 1s**.

---

## 🔹 3. Why Do We Use the Decimal System?

Even though computers use **binary**, humans prefer **decimal (0–9)** because:
1. **Human-friendly** → Our counting system is based on 10 fingers → natural for us.
2. **Compact representation** → Writing `1100100` (binary) is harder than writing `100` (decimal).
3. **Everyday math** → Finance, measurement, etc. use decimal, so computers need conversions.

### Example:
- Binary: `1010`  
- Decimal: `10`  
- Much easier for humans to read/interpret decimal.

---

## 🔹 Relationship Between Binary & Decimal
- Binary (base 2) → computer’s language.  
- Decimal (base 10) → human’s language.  
- Conversion is done automatically by software, compilers, and hardware instructions.

### Example:
- Decimal `13` = Binary `1101`.  
- When you type `13` in a program, it’s converted into binary before the CPU processes it.

---

## ✅ Summary

- At the **physical level**, data = 0s and 1s (electricity, magnetism, light, radio waves).  
- **Storage** → HDD (magnetism), SSD (transistors), RAM (capacitors), CPU (transistors).  
- **Transfer** → electrical signals, light pulses, radio waves.  
- We **use decimal** because it’s easier for humans, but computers always work in binary internally.  

---
