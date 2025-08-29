# ✨ I/O Ports of 8051 in Parallel Communication

---

## 📌 1. Introduction

* 8051 has **4 parallel I/O ports**: **P0, P1, P2, P3**, each **8-bit wide** (total 32 I/O lines).
* Each port can be programmed as **input** or **output**.
* Parallel communication means **sending/receiving multiple bits (8-bits) simultaneously**.
* Each port has a **latch (D flip-flop), output driver (FET), and input buffer**.

---

## 📌 2. Port Functioning in **Input Mode**

1. Write logic **1** to the port latch (by `MOV Pn, #FFH`).
2. This turns OFF the output FET (open drain).
3. Pin status is then determined by the **external device** (input data).
4. CPU reads pin value through the input buffer.

✅ Example (Read from Port 1):

```asm
MOV P1, #0FFH   ; Configure Port1 as input
MOV A, P1       ; Read 8-bit data from P1 into A
```

---

## 📌 3. Port Functioning in **Output Mode**

1. Write logic **0** or **1** to the port latch.
2. Output driver sends that value to the pin.
3. Pin drives external device directly.

✅ Example (Write to Port 2):

```asm
MOV P2, #0A5H   ; Send data 10100101 to Port2 pins
```

---

## 📌 4. Analysis of All Ports

### 🔹 (i) **Port 0 (P0.0 – P0.7)**

* **Dual role**: General-purpose I/O or **multiplexed address/data bus (AD0–AD7)** for external memory.
* **Open-drain output** → needs **external pull-up resistors** for output operations.
* As input: Must write `1` before reading.
  ✅ Example: `MOV A, P0` (reads external data bus).

---

### 🔹 (ii) **Port 1 (P1.0 – P1.7)**

* **Dedicated I/O port only** (no alternate function).
* Has **internal pull-ups** → no need for external resistors.
* Most reliable for general input/output.
  ✅ Example: `MOV P1, #0FFH` → configure as input.

---

### 🔹 (iii) **Port 2 (P2.0 – P2.7)**

* General-purpose I/O OR higher-order address bus (A8–A15) in external memory interfacing.
* Has **internal pull-ups**.
* Useful for driving external memory or latches.
  ✅ Example: `MOV P2, #55H` → send data pattern to Port2.

---

### 🔹 (iv) **Port 3 (P3.0 – P3.7)**

* Dual-role pins with **special alternate functions**:

| Pin  | Function                       |
| ---- | ------------------------------ |
| P3.0 | RXD (Serial input)             |
| P3.1 | TXD (Serial output)            |
| P3.2 | INT0 (External interrupt 0)    |
| P3.3 | INT1 (External interrupt 1)    |
| P3.4 | T0 (Timer 0 input)             |
| P3.5 | T1 (Timer 1 input)             |
| P3.6 | WR (Write to external memory)  |
| P3.7 | RD (Read from external memory) |

* Has **internal pull-ups**.
* Can be used as input/output if not used for alternate functions.

✅ Example:

```asm
MOV P3, #0F0H   ; Upper nibble = 1, Lower nibble = 0
```

---

## 📌 5. Comparative Table

| Port   | Input Mode                       | Output Mode                   | Special Features                                                      |
| ------ | -------------------------------- | ----------------------------- | --------------------------------------------------------------------- |
| **P0** | Requires external pull-ups       | Open-drain output             | Multiplexed AD0–AD7                                                   |
| **P1** | Simple input (internal pull-ups) | Simple output                 | Pure I/O                                                              |
| **P2** | With internal pull-ups           | Output with internal pull-ups | Acts as A8–A15 for external memory                                    |
| **P3** | With internal pull-ups           | Output with internal pull-ups | Has **special functions** (serial, timer, interrupts, memory control) |

---

## 📌 6. Conclusion

* All 4 ports (P0–P3) support **parallel communication (8 bits at a time)**.
* **P0 & P2** are special: also used for **external memory interfacing**.
* **P1** is the simplest, dedicated I/O.
* **P3** doubles as **control/serial/interrupt port**.
* Together, they make 8051 suitable for **interfacing with external devices, memory, and peripherals**.

---

✅ **In exam (12 marks distribution):**

* Intro (1–2M)
* Input/Output working (2M)
* Each port explained (5M)
* Table + Conclusion (3–4M)
