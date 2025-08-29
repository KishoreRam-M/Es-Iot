# 🔹 SMOD Bit in 8051

The **SMOD (Serial Mode) bit** is located in the **PCON (Power Control) register** of the 8051.
It controls the **baud rate** (speed) of the UART (serial port).

---

## 📌 1. Register

* The SMOD bit belongs to the **PCON register**.
* PCON contains control bits for power modes and serial communication speed.

**PCON Register Layout:**

| Bit | 7 (SMOD) | 6 | 5 | 4   | 3   | 2  | 1   | 0 |
| --- | -------- | - | - | --- | --- | -- | --- | - |
| Use | Baud ×2  | — | — | GF1 | GF0 | PD | IDL | — |

---

## 📌 2. Power-up Status

* On **reset / power-up**, **SMOD = 0 (cleared)**.
* This means UART runs at **normal baud rate**.

---

## 📌 3. Function (Detailed)

* SMOD controls the **UART baud rate multiplier**.

### 🔹 When SMOD = 0

Baud Rate =

$$
\frac{\text{Timer Overflow Frequency}}{32}
$$

👉 Normal speed (default).

### 🔹 When SMOD = 1

Baud Rate =

$$
\frac{\text{Timer Overflow Frequency}}{16}
$$

👉 Baud rate is **doubled automatically**.

---

### ✅ Why useful?

* Helps achieve **standard baud rates** (like 9600 bps, 19200 bps).
* Useful when the timer cannot exactly match the required baud rate.
* Allows **faster communication** with PCs, modems, or sensors.

---

## 📌 Example

Suppose Timer generates a frequency for **4800 bps** when SMOD = 0.

* With **SMOD = 0 → Baud = 4800 bps**
* With **SMOD = 1 → Baud = 9600 bps** (doubled)

---

## ✨ In Short

* **Register:** SMOD is in PCON.
* **Status:** 0 at reset (normal speed).
* **Function:** When set to 1, **doubles UART baud rate**.
