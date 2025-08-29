Got it 👍 — since you only want the **Registers part** (not the block/pin diagrams), I’ll give you a **full, exam-ready section on Registers of 8051**.

---

# **Registers of 8051 Microcontroller**

Registers are small, high-speed storage locations inside the CPU used for data storage, instruction execution, and control. The **8051 has several special and general-purpose registers:**

---

## **1. Accumulator (A)**

* 8-bit register.
* Used in almost all arithmetic, logic, and data transfer operations.
* Example: `ADD A, R1` → adds R1 to A.

---

## **2. B Register**

* 8-bit register.
* Used with the Accumulator for multiplication and division.

  * Example: `MUL AB` → result: A = lower byte, B = higher byte.

---

## **3. Program Status Word (PSW)**

* 8-bit flag register.
* Bits:

  * **CY (Carry)**
  * **AC (Auxiliary Carry)**
  * **OV (Overflow)**
  * **P (Parity)**
  * **RS0, RS1** → select register banks (Bank 0–3).

---

## **4. Program Counter (PC)**

* 16-bit register.
* Holds the address of the **next instruction** to execute.
* Increments automatically during program execution.

---

## **5. Stack Pointer (SP)**

* 8-bit register.
* Points to the **top of stack** in RAM.
* Default value = 07H (so stack begins at 08H).

---

## **6. Data Pointer (DPTR)**

* 16-bit register, divided into DPH (high) and DPL (low).
* Used for **external memory addressing**.

---

## **7. General Purpose Registers (R0–R7)**

* Located in the **internal RAM**.
* Organized into 4 banks (Bank 0–3).
* Each bank has 8 registers → total 32 registers.
* Selected using **RS0 & RS1 bits of PSW**.

---

# **Summary Table — Registers of 8051**

| Register        | Size                 | Function                         |
| --------------- | -------------------- | -------------------------------- |
| A (Accumulator) | 8-bit                | Arithmetic, logic, data transfer |
| B               | 8-bit                | Multiply/Divide with A           |
| PSW             | 8-bit                | Flags + bank selection           |
| PC              | 16-bit               | Next instruction address         |
| SP              | 8-bit                | Stack top pointer                |
| DPTR            | 16-bit               | External memory addressing       |
| R0–R7           | 8-bit × 8 (×4 banks) | General purpose                  |

---

✅ That’s the **Registers section only** — clean, exam-ready, and scoring full marks.

Do you want me to also make a **short mnemonic** to easily remember all 7 types of registers right before the exam?
