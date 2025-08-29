# ⏱️ Timer Modes in 8051 Microcontroller

The **8051 microcontroller** has **two 16-bit timers/counters (Timer0 and Timer1)**.
Each timer consists of **THx (high byte)** and **TLx (low byte)** registers.

Timers are controlled by:

* **TMOD (Timer Mode Register)** → Selects mode (M1, M0)
* **TCON (Timer Control Register)** → Controls start/stop and overflow flags

---

## 🔹 Control Bits

* **TMOD Register**:

  * M1, M0 → Mode select
  * C/T → Counter/Timer select (0 = timer, 1 = counter)
  * GATE → Hardware gating control

* **TCON Register**:

  * TRx → Run control bit (Start/Stop)
  * TFx → Timer overflow flag

---

## 🔹 Timer Modes Overview

| **Mode** | **Type**          | **Size**                   | **Range** | **Special Feature**  | **Usage**                |
| -------- | ----------------- | -------------------------- | --------- | -------------------- | ------------------------ |
| Mode 0   | 13-bit            | TLx = 8 bits, THx = 5 bits | 0 → 8191  | Obsolete/short timer | Rarely used              |
| Mode 1   | 16-bit            | TLx + THx = 16 bits        | 0 → 65535 | Full timer           | **Delays, counters**     |
| Mode 2   | 8-bit auto reload | TLx = 8 bits, THx = reload | 0 → 255   | Auto reload from THx | **Baud rate generation** |
| Mode 3   | Split             | Timer0 → 2 x 8-bit timers  | 0 → 255   | Only for Timer0      | Get 3 timers total       |

---

## 🔹 Mode 0 → 13-bit Timer

* **THx (5 bits used)** + **TLx (8 bits)** = 13-bit timer.
* Range: `0000H → 1FFFH (8191 decimal)`

📖 Think: *Short counter, not practical today*.

**Diagram:**

```
THx (5 bits) ---> TLx (8 bits) ---> Overflow → TFx
```

---

## 🔹 Mode 1 → 16-bit Timer

* **THx (8 bits)** + **TLx (8 bits)** = 16-bit timer.
* Range: `0000H → FFFFH (65535 decimal)`

📖 Think: *Normal full timer, widely used*.

**Diagram:**

```
THx (8 bits) ---> TLx (8 bits) ---> Overflow → TFx
```

✅ Best for **long delays** and **general timing**.

---

## 🔹 Mode 2 → 8-bit Auto-Reload Timer

* **TLx (8 bits)** → Counts.
* **THx (8 bits)** → Stores reload value.
* On overflow, TLx auto-reloads from THx.

📖 Think: *Self-resetting short timer*.

**Diagram:**

```
THx (Reload Value) → TLx (Counts) → Overflow → TFx
                        ↑
                        └─── Auto reload from THx
```

✅ Best for **UART baud rate generation**.

---

## 🔹 Mode 3 → Split Timer Mode

* **Only for Timer0**.
* Splits Timer0 into:

  * **TL0 → 8-bit timer (with TR0, TF0)**
  * **TH0 → 8-bit timer (with TR1, TF1)**

📖 Think: *One timer becomes two*.

**Diagram:**

```
Timer0 Split:
   TH0 (8 bits) → Overflow → TF1
   TL0 (8 bits) → Overflow → TF0
```

✅ Effect: Gives **three timers total** (TH0, TL0, Timer1).

---

## 🔹 Usage Notes

* **Mode 0** → Obsolete (short counter).
* **Mode 1** → Long delays, most common.
* **Mode 2** → Repeated auto-reload (UART baud).
* **Mode 3** → Split Timer0 into 2 × 8-bit timers.

---

## ✨ Summary (One-Liner Mnemonics)

* **Mode 0** → 13-bit → *Rare*
* **Mode 1** → 16-bit → *Normal delay*
* **Mode 2** → 8-bit Auto Reload → *UART baud*
* **Mode 3** → Split → *Timer0 = 2 small timers*
