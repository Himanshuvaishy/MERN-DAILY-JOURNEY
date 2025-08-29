# Introduction to Data Representation in Computing  

Computers are powerful machines, but at their core, they understand only **two states: ON and OFF**. These are represented using **binary digits (bits)**:  
- `1` → ON (high voltage)  
- `0` → OFF (low voltage)  

Since computers operate on binary, all types of information — numbers, text, images, audio, and video — must be converted into a **binary form** before a computer can process it. This process is called **data representation**.  

---

## Why Do We Need Data Representation?  
- To **store information** in memory or storage devices.  
- To **process data** using logical and arithmetic operations.  
- To **transmit information** between devices and systems.  

---

## Basic Units of Data  
1. **Bit (Binary Digit):** Smallest unit, can be `0` or `1`.  
2. **Nibble:** 4 bits (e.g., `1010`).  
3. **Byte:** 8 bits (e.g., `01010101`).  
4. **Kilobyte (KB):** 1024 bytes.  
5. **Megabyte (MB), Gigabyte (GB), Terabyte (TB):** Larger storage units.  

---

## Types of Data Representation  

1. **Numbers:**  
   - **Binary System (Base-2):** Uses `0` and `1`.  
   - **Decimal System (Base-10):** Our normal number system (0–9).  
   - **Octal (Base-8) & Hexadecimal (Base-16):** Used as shorthand for binary.  

2. **Characters (Text):**  
   - **ASCII (American Standard Code for Information Interchange):** Represents English letters, digits, and symbols (e.g., `A = 65`).  
   - **Unicode:** Universal standard to represent characters from all languages (e.g., `अ`, `あ`, `ع`).  

3. **Images:**  
   - Stored as a grid of **pixels**, each pixel represented by binary values (e.g., RGB values).  

4. **Audio:**  
   - Sound waves are **sampled** at intervals and converted to binary numbers.  

5. **Video:**  
   - Sequence of images (frames) + audio, both represented in binary.  

---

## Example: Representing the letter `A`  
- ASCII code of `A` = 65 (in decimal).  
- In binary: `01000001`.  
- Stored in computer memory as 8 bits (1 byte).  

---

✅ **In short:** Data representation is the way computers store and process all forms of information using **binary (0s and 1s)**.  
