# ✨ Serial Communication in 8051

The **8051 microcontroller** supports both **serial transmission (Tx)** and **serial reception (Rx)** using **UART (Universal Asynchronous Receiver/Transmitter)**.

* Serial communication allows data to be transmitted **bit by bit** using two pins:

  * **TXD (P3.1)** → Transmit Data
  * **RXD (P3.0)** → Receive Data

* Unlike parallel communication (8 bits at once), serial is used for **long-distance, low-pin communication** (e.g., RS-232).

---

## 🔹 Modes of Serial Communication (Selected by SCON)

8051 UART has **4 modes**:

| **Mode** | **Bits**                            | **Baud Rate**  | **Use Case**                |
| -------- | ----------------------------------- | -------------- | --------------------------- |
| Mode 0   | 8-bit shift register                | Fixed (Osc/12) | Simple shift register       |
| Mode 1   | 10-bit UART (8 data + start + stop) | Variable       | Standard UART               |
| Mode 2   | 11-bit UART (9 data + start + stop) | Fixed          | Multiprocessor comm.        |
| Mode 3   | 11-bit UART                         | Variable       | Multiprocessor, custom baud |

---

## 🔹 Serial Communication Process

### Transmission (Tx)

1. CPU writes data into **SBUF register**.
2. Data is shifted out bit by bit via **TXD pin**.
3. **TI (Transmit Interrupt flag in SCON)** is set when transmission is complete.

### Reception (Rx)

1. Serial data arrives at **RXD pin**.
2. Data is shifted into UART buffer.
3. Byte is copied into **SBUF register**.
4. **RI (Receive Interrupt flag in SCON)** is set → CPU reads SBUF.

---

## 🔹 Role of Key Registers

### 1. **SBUF (Serial Buffer Register)**

* Acts as a **data register for both Tx and Rx**.
* Write to SBUF → data goes to transmit shift register.
* Read from SBUF → received data is read.

```assembly
MOV SBUF, #'A'   ; Transmit character 'A'
JNB TI, $        ; Wait until transmission complete
CLR TI           ; Clear flag
```

```assembly
JNB RI, $        ; Wait until data received
MOV A, SBUF      ; Read received data into Accumulator
CLR RI           ; Clear flag
```

---

### 2. **SCON (Serial Control Register)**

Controls and monitors serial port.

| Bit | Name    | Function                             |
| --- | ------- | ------------------------------------ |
| 7   | **SM0** | Mode select                          |
| 6   | **SM1** | Mode select                          |
| 5   | **SM2** | Multiprocessor mode enable           |
| 4   | **REN** | Reception enable (1 = allow receive) |
| 3   | **TB8** | 9th bit transmitted in Modes 2/3     |
| 2   | **RB8** | 9th bit received in Modes 2/3        |
| 1   | **TI**  | Transmit interrupt flag              |
| 0   | **RI**  | Receive interrupt flag               |

➡️ **Key roles:**

* Selects serial mode (SM0, SM1).
* Enables reception (REN).
* Monitors completion of Tx (TI) and Rx (RI).

---

### 3. **PCON (Power Control Register)**

* Contains **SMOD (Serial Mode Control Bit)**.
* If **SMOD = 1**, baud rate is **doubled**.

| Bit | Name | Function             |
| --- | ---- | -------------------- |
| 7   | SMOD | Baud rate double bit |
| 6–1 | —    | Power control bits   |
| 0   | IDL  | Idle mode control    |

➡️ **Effect:** Adjusts baud rate without changing Timer reload values.

---

## 🔹 Example: Serial Transmission (Mode 1, 9600 Baud)

```assembly
MOV TMOD, #20H      ; Timer1, Mode2 (8-bit auto-reload)
MOV TH1, #-3        ; Reload value for 9600 baud (assuming 11.0592 MHz)
MOV SCON, #50H      ; Mode1, REN=1 (8-bit UART)
SETB TR1            ; Start Timer1

; Transmit 'A'
MOV SBUF, #'A'      
JNB TI, $           
CLR TI              ; Clear flag after transmission

; Receive data
JNB RI, $           
MOV A, SBUF         ; Store received byte
CLR RI              ; Clear receive flag
```

---

## 🔹 Diagram of Serial Communication

```
   CPU -----> [ SBUF ] -----> [ Tx Shift Reg ] -----> TXD pin
                ^
                |
                v
   RXD pin <----[ Rx Shift Reg ] <---- [ SBUF ] <---- CPU
```

Flags monitored through **SCON**, baud rate adjusted using **PCON**.

---

## ✨ Final Exam Answer Summary

* **Serial communication** in 8051 uses TXD and RXD pins with UART.
* **SBUF** → Buffer register for Tx (write) and Rx (read).
* **SCON** → Configures mode, enables Rx, monitors Tx (TI) and Rx (RI).
* **PCON** → Controls baud rate (SMOD = 1 doubles baud).
* Transmission: CPU writes to SBUF → data shifted via TXD.
* Reception: RXD receives → data stored in SBUF → RI flag set.
