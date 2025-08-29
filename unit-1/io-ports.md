# ✨ Functioning of 8051 I/O Ports in Parallel Communication (Input & Output Modes)

The **8051 microcontroller** has **4 parallel I/O ports (P0, P1, P2, P3)**.

* Each port is **8-bit wide**, so total **32 I/O pins**.
* Each port can be programmed as **input or output**.
* Ports are **bit-addressable** (each bit can be controlled individually).

---

## 🔹 General Mechanism of I/O Ports

* **Output Mode**: Write data into port latch → appears at pins.
* **Input Mode**: Write logic ‘1’ (FFH) to latch → pin reads external signal.
* Ports may have **internal pull-ups** (except P0).

---

## 1. **Port 0 (P0.0 – P0.7)**

* **Unique Feature:** It is a **dual-purpose port** →

  * General purpose I/O
  * **Multiplexed address/data bus (AD0–AD7)** in external memory interfacing.
* **Pull-ups:** No internal pull-ups (requires external).

**Output Mode:**

```assembly
MOV P0, #55H   ; Output 55H on Port 0
```

* Data is sent to external world.
* If external memory used → serves as multiplexed low byte address/data.

**Input Mode:**

```assembly
MOV P0, #0FFH  ; Configure as input
MOV A, P0      ; Read data from port pins
```

* Writing `1` makes pins act as input.

---

## 2. **Port 1 (P1.0 – P1.7)**

* **Dedicated I/O port only** (no alternate function).
* Has **internal pull-ups**, making it easy to use.

**Output Mode:**

```assembly
MOV P1, #0A5H  ; Output A5H on Port 1
```

**Input Mode:**

```assembly
MOV P1, #0FFH  ; Configure as input
MOV A, P1      ; Read external data into A
```

---

## 3. **Port 2 (P2.0 – P2.7)**

* **Dual-purpose port** →

  * General purpose I/O
  * **High byte of address bus (A8–A15)** in external memory interfacing.
* Has internal pull-ups.

**Output Mode:**

```assembly
MOV P2, #0F0H  ; Send F0H to Port 2
```

**Input Mode:**

```assembly
MOV P2, #0FFH  ; Configure as input
MOV A, P2      ; Read value from port
```

---

## 4. **Port 3 (P3.0 – P3.7)**

* **Special Function Port** → Each pin has alternate functions:

| **Pin** | **Alternate Function**      |
| ------- | --------------------------- |
| P3.0    | RXD (Serial input)          |
| P3.1    | TXD (Serial output)         |
| P3.2    | INT0 (External interrupt 0) |
| P3.3    | INT1 (External interrupt 1) |
| P3.4    | T0 (Timer 0 external input) |
| P3.5    | T1 (Timer 1 external input) |
| P3.6    | WR (External memory write)  |
| P3.7    | RD (External memory read)   |

* Can still be used as **general I/O** if alternate functions not enabled.

**Output Mode:**

```assembly
MOV P3, #0AAH  ; Output AAH to Port 3
```

**Input Mode:**

```assembly
MOV P3, #0FFH  ; Configure as input
MOV A, P3      ; Read input from pins
```

---

## 🔹 Comparison of Ports (Input vs Output in Parallel Communication)

| **Port** | **Pull-up** | **Alternate Use**                              | **Output Use**                   | **Input Use**                   |
| -------- | ----------- | ---------------------------------------------- | -------------------------------- | ------------------------------- |
| **P0**   | No          | AD0–AD7 (addr/data)                            | Data out, needs external pull-up | Set FFH → reads external input  |
| **P1**   | Yes         | None                                           | Simple output                    | Simple input                    |
| **P2**   | Yes         | A8–A15 (address)                               | Sends high addr/data             | Reads inputs                    |
| **P3**   | Yes         | Serial, interrupts, timers, read/write control | Output data or control signals   | Input data or interrupt signals |

---

## 🔹 Example: Parallel Communication (Input & Output)

```assembly
MOV P1, #0FFH    ; Set Port1 as input
MOV A, P1        ; Read data from external device into A
MOV P2, A        ; Output same data on Port2 (parallel output)
```

* Here Port1 acts as **parallel input port**.
* Port2 acts as **parallel output port**.

---

## ✨ Final Exam Answer Summary

* The 8051 has **4 parallel I/O ports (P0–P3)**, each **8 bits wide**.
* **Port 0 & 2** → dual purpose (used in external memory interfacing).
* **Port 1** → pure I/O (easiest for parallel input/output).
* **Port 3** → multifunction (serial, interrupts, timers, read/write) + I/O.
* **Input mode**: write `1` (FFH) → read external signals.
* **Output mode**: write data to port latch → appears on pins.
* Example code shows parallel communication between ports.

