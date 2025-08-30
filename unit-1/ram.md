### **Introduction**

The 8051 microcontroller has only **128 bytes of internal RAM**, which is too small to store long data like strings.
Therefore, it allows connection of **external memory (XDATA)**. By connecting a **32 KB RAM chip** to the 8051, we can store and retrieve strings (series of characters) easily.

---

### **Main Explanation**

To connect the RAM properly, we must wire **address lines, data lines, and control signals**.

#### **1. Address Bus**

* Every memory location in RAM has an **address** (like a cupboard with drawers).
* 32 KB RAM needs **15 address lines (A0–A14)**.
* **Port 0 (P0)** of 8051 gives the **lower 8 address bits (A0–A7)**, but it is multiplexed with data.
* So we use a **Latch (74HC373)** with the **ALE signal** to lock these lower address bits.
* **Port 2 (P2)** gives the **higher address bits (A8–A14)**.

#### **2. Data Bus**

* RAM has **8 data pins (D0–D7)**.
* These are connected directly to **Port 0 (P0)** of 8051 (shared lines).
* After address is latched, Port 0 carries data (the character byte).

#### **3. Control Signals**

* **/RD (Read)** of 8051 → connected to **/OE (Output Enable)** of RAM.
* **/WR (Write)** of 8051 → connected to **/WE (Write Enable)** of RAM.
* **A15** line (from P2.7) → used for **Chip Select (/CS)** so that RAM is active only in range **0000h–7FFFh**.

#### **4. Operation (Storing a String)**

* Suppose we want to store “HELLO”:

  1. 8051 loads starting address (say 2000h) into its **DPTR register**.
  2. First character ‘H’ is placed on the data bus.
  3. **/WR** is made low → byte written into RAM at 2000h.
  4. DPTR is incremented → next address 2001h.
  5. Next character ‘E’ written, and so on.
* Thus, each character of the string occupies **one memory location**.

---

### **Diagram (Simplified ASCII)**

```
         +------------------+        +------------------+
P2.0–2.6 | A8–A14           |        |                  |
         |                  |        |   32KB RAM       |
P2.7 --->| A15 ---[INV]---> | /CS    |   A0–A14, D0–D7  |
         |                  |        |   /OE, /WE       |
         +------------------+        +------------------+
              |        |
8051 P0 ----->| Latch  |---> A0–A7
  (AD0–AD7)   | (74HC373)
              |
              +----------------------> D0–D7 (RAM data bus)

8051 ALE ---------------------------> Latch Enable  
8051 /RD ---------------------------> RAM /OE  
8051 /WR ---------------------------> RAM /WE
```

---

### **Key Points**

* **Port 0**: carries **low address (A0–A7)** and **data (D0–D7)**, needs latch.
* **Port 2**: carries **high address (A8–A14)**.
* **ALE**: latches the low address.
* **/RD, /WR**: control signals for read/write.
* **A15 decode**: ensures RAM is selected only in correct range.
* **String storage**: one character per memory location, stored sequentially.

---

### **Conclusion**

The 8051 accesses external memory using a **multiplexed address/data bus**. By connecting a **32 KB RAM chip** through **Port 0, Port 2, ALE, /RD, and /WR**, we can store strings of characters in external memory.
Each character is stored in consecutive addresses, making it easy to write and later retrieve the entire string.
