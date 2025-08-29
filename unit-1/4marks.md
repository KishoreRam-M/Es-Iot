# Part-B — Model answers (Q1 → Q16)

---

## 1. Describe the working of a microcontroller and illustrate its real-time applications in embedded systems. (CO1.1 — CL2)

**Definition (1 line):**
A **microcontroller** is a single-chip computer that integrates CPU, memory (ROM/Flash & RAM), and peripherals (timers, serial I/O, GPIO) designed to perform dedicated control tasks in embedded systems.

**Working (3–4 lines):**

* The CPU fetches instructions from program memory (on-chip ROM/Flash), decodes and executes them.
* Data is read/written to internal RAM or peripherals via internal buses; I/O pins interface with sensors/actuators.
* Timers, interrupts and serial interfaces enable real-time responses: interrupts suspend normal flow to service time-critical events.

**Real-time applications (3–4 lines / examples):**

* **Automotive:** Engine control (ECU) using sensor feedback and strict timing.
* **Industrial automation:** PLC-like control for conveyor belts responsive to interrupts.
* **Consumer electronics:** Microwave oven controllers, washing machine cycles.
* **IoT nodes:** Sensor sampling + low-power sleep/wake cycles.

**Concluding note:** microcontrollers are tailored to real-time control due to integrated peripherals and fast interrupt handling.

---

## 2. Compare and analyze the architectural and functional differences between a microprocessor and a microcontroller with suitable examples. (CO1.1 — CL4)

**Comparison table (concise):**

| Feature     |           Microprocessor (MPU) | Microcontroller (MCU)                     |
| ----------- | -----------------------------: | ----------------------------------------- |
| Integration |                       CPU only | CPU + RAM + ROM + Peripherals on one chip |
| Typical use | General-purpose computing, PCs | Embedded control, appliances              |
| Peripherals |        External (separate ICs) | On-chip timers, UART, ADC, GPIO           |
| Cost & Size |   Higher, needs external chips | Lower, compact single-chip solution       |
| Example     | Intel i7 (needs chipset, DRAM) | 8051, AVR, PIC (single-chip controllers)  |

**Analysis (2–3 lines):**
MPUs are optimized for throughput and complex OSs; MCUs are optimized for deterministic control, low power, and cost. Choose MPU for complex apps (desktop/server), MCU for dedicated embedded tasks (sensor nodes, controllers).

---

## 3. Analyze the architecture of the 8051 microcontroller and explain the function of its key components with a neat labeled diagram. (CO1.2 — CL3)

**Intro (1 line):**
8051 is an 8-bit Harvard-architecture microcontroller with on-chip ROM, RAM, timers, serial port and I/O ports.

**How to draw the diagram (do this on the answer sheet):**
Sketch boxes and label: CPU/ALU → Accumulator (A), B register; Program Counter (PC); Stack Pointer (SP); Data Pointer (DPTR, 16-bit); Internal RAM (00–7F), SFR block (80–FF); Timers (T0,T1); Serial control (UART/SCON/SBUF); Interrupt controller; Ports P0–P3; Oscillator & reset.

**Key components & functions (bulleted):**

* **ALU & Accumulator (A):** Performs arithmetic/logic; A is primary operand register.
* **DPTR (16-bit):** Used for external memory addressing (MOVX, MOVC).
* **Program Counter (PC):** Points to next instruction in code memory.
* **Stack Pointer (SP):** Points to top of internal stack in RAM (reset = 07H).
* **Register banks (R0–R7):** Four banks selected by PSW\.RS1\:RS0.
* **SFRs:** Control peripherals (TCON, TMOD, SCON, SBUF, IE, IP, PCON, PSW).
* **Timers/Counters:** Timer0/1 for timing/delays and event counting.
* **Serial port:** Full duplex UART (SCON, SBUF) for serial comms.
* **Interrupt block:** Five vectored interrupts with priority control.

**Conclusion:** Understanding these blocks + SFR roles is essential for programming and interfacing.

---

## 4. Illustrate and explain the pin diagram of the 8051 microcontroller by applying your understanding of its internal components and their functions. (CO1.2 — CL2)

**How to draw (do on paper):**
Draw a 40-pin DIP rectangle and label the following groups of pins with brief notes:

* **P0.0–P0.7 (Port 0):** Multiplexed AD0–AD7 for external data/low address; requires pull-ups.
* **P1.0–P1.7 (Port 1):** General purpose I/O.
* **P2.0–P2.7 (Port 2):** High address byte for external memory (A8–A15).
* **P3.0–P3.7 (Port 3):** Alternate functions: RxD/TxD (serial), INT0/INT1 (external interrupts), T0/T1 (timer inputs), WR/RD (external data memory control).
* **ALE (Address Latch Enable):** Latches low address byte for external memory.
* **PSEN (Program Store Enable):** Signals external program memory read.
* **EA\ (External Access Enable):** When low, fetches code from external memory.
* **VCC & GND, XTAL1/XTAL2 (oscillator), RST (reset).**

**Explanation (brief):**
Each pin either serves as general I/O or has a specific alternate function used when interfacing to external memory and peripherals. Labeling alternate functions in the diagram gains marks.

---

## 5. List the different types of addressing modes used in the 8051 microcontroller and give example for any two of the addressing modes. (CO1.3 — CL2)

**Addressing modes (list):**

1. Immediate (`#data`)
2. Register (`R0`..`R7`)
3. Direct (direct memory / SFR address)
4. Register indirect (`@R0`, `@R1`)
5. Indexed (code memory: `MOVC A,@A+DPTR`)
6. Absolute for external memory (`MOVX` variants)

**Examples (two):**

* **Immediate:** `MOV A, #45H` — loads the constant 45H into A.
* **Register indirect:** `MOV A, @R0` — loads A with the byte addressed by the register R0 (useful for arrays in internal RAM).

---

## 6. Develop a program to add two 8-bit numbers stored at memory locations 40H and 41H, and store the result at 42H. (CO1.3 — CL3)

**Algorithm (1–2 lines):**
Load first operand into accumulator, add second operand, store result at destination.

**8051 Assembly (annotated):**

```asm
; Add contents of 40H and 41H, store result at 42H
    MOV A, 40H     ; A <- [40H]
    ADD A, 41H     ; A <- A + [41H]  ; CY/AC/P affected
    MOV 42H, A     ; [42H] <- A (result)
    ; Optionally check CY if you want to note overflow
```

**Trace example:** if 40H = 10H, 41H = 20H → 42H = 30H. If sum > 0xFF, CY = 1 and A contains LSB.

---

## 7. Write an assembly language program to toggle an LED connected to Port 1.0 with a 1-second delay. (CO1.3 — CL3)

**Method A — Simple software delay (assume moderate clock):**

```asm
; Toggle P1.0 with approximate 1s delay (software delay)
MAIN:
    CLR P1.0         ; Ensure LED initial state (or use CPL to toggle directly)
LOOP:
    CPL P1.0         ; Toggle P1.0
    ACALL DELAY_1S   ; Call software 1s delay (calibrated for target crystal)
    SJMP LOOP

;-------------------------
DELAY_1S:
    ; Nested loops — tune counts per your MCU clock
    ; Example placeholders — calibrate these constants for exact 1s on your board.
    MOV R2, #200
D1: MOV R1, #250
D2: MOV R0, #200
D3: DJNZ R0, D3
    DJNZ R1, D2
    DJNZ R2, D1
    RET
```

*Note:* The constants (200, 250, 200) must be calibrated for your oscillator frequency. Use measurement or timer method for exact 1s.

**Method B — Recommended: Timer0 in Mode 1 (16-bit) with overflow counting**
(Outline & steps — exam answer should show method and formula.)

1. **Assume crystal frequency** (state it; common exam choice is 11.0592 MHz). Machine cycles/sec = crystal/12.
2. **Compute required timer ticks for 1s:** `Ticks = crystal/12`. For 11.0592MHz → `Ticks = 921,600`. Since 921,600 > 65536, count multiple overflows: `overflows = floor(Ticks/65536) = 14` plus remainder.
3. **Set TMOD = 01H** (Timer0 mode1), load TH0/TL0 for initial value `(65536 − remainder)`, start TR0 and count overflows in a loop or use TF0 interrupt and count until desired number reached.
4. **On each overflow toggle P1.0 when desired number of overflows reached.**

**Conclusion:** Method B is accurate and preferred for exam/real system; in answers show the calculation steps and TMOD/TCON/SFR settings.

---

## 8. Differentiate between `MOV A, #45H` and `MOV A, 45H` with respect to addressing modes. (CO1.3 — CL4)

**`MOV A, #45H` — Immediate addressing:**

* The value `45H` is part of the instruction and is loaded directly into A. No memory access. Use for constants.

**`MOV A, 45H` — Direct addressing:**

* The operand `45H` is treated as an internal RAM/SFR address; the byte stored at memory location 45H is loaded into A. Use to fetch variable stored in RAM.

**Example:** `MOV A, #05H` → A = 05H. `MOV A, 30H` → A = contents of RAM\[30H].

---

## 9. Examine the role of the PSW register during arithmetic operations and its impact on program logic. (CO1.3 — CL4)

**PSW (Program Status Word) — roles (bulleted):**

* **Carry (CY):** Reflects carry out of MSB — used for multi-byte arithmetic and conditional branching (e.g., `JNC`, `JC`).
* **Auxiliary Carry (AC):** Set on carry from bit3 to bit4 — useful in BCD adjustments.
* **Overflow (OV):** Indicates signed overflow for arithmetic operations.
* **Parity (P):** Parity of accumulator (automatically updated).
* **Bank select bits (RS1\:RS0):** Select register bank (R0–R7 sets).
* **User flag F0:** General purpose.

**Impact on program logic (2–3 lines):**
Flags determine branching and multi-byte arithmetic behavior. For example, when adding two bytes with carry to form a 16-bit sum, the CY is saved and used in the next add (`ADDC A, #data`). OV can affect signed arithmetic decisions, and P may be used for parity checks in communication.

---

## 10. Compare the efficiency of parallel and serial communication in the context of the 8051 microcontroller. (CO1.4 — CL4)

**Comparison (concise):**

| Aspect                 |                         Parallel | Serial                                                                 |
| ---------------------- | -------------------------------: | ---------------------------------------------------------------------- |
| Lines required         | Multiple data lines (8 for byte) | Single/differential pair + control lines                               |
| Speed (short distance) |         High (simultaneous bits) | Lower per-wire but modern serial high-speed protocols can match/exceed |
| Cost & wiring          |           More pins, wires, cost | Fewer lines, cheaper, simpler routing                                  |
| Distance               |   Poor over long distance (skew) | Better for long distances (UART, RS-232, RS-485)                       |
| 8051 use-case          |  On-chip peripheral or local bus | UART/RS-232 for external comm and sensors                              |

**Conclusion:** For short, local transfers (on PCB) parallel is fast but costly; for external communication and simpler wiring serial is more efficient and common with 8051 (using SCON/SBUF).

---

## 11. List the advantages and disadvantages of parallel communication over serial communication in 8051. (CO1.4 — CL2)

**Advantages (parallel):**

* Faster per-byte transfer (all bits transmitted simultaneously).
* Simpler to implement if many pins available for local high-speed transfers.

**Disadvantages (parallel):**

* Requires many I/O pins and more wiring, increasing cost and PCB complexity.
* Signal skew limits distance; more susceptible to crosstalk over long cables.

**Use in 8051:** Parallel useful for local external memory/data buses; serial (UART) preferred for external peripheral comms.

---

## 12. Which register has the SMOD bit, and what is its status when the 8051 is powered up? (CO1.4 — CL2)

**Answer (concise):**

* **Register:** `PCON` (Power Control register) contains the **SMOD** bit.
* **Status at power-up:** **SMOD = 0** (cleared) by default. When set to 1, SMOD doubles UART baud rate (when using appropriate timer source).

---

## 13. Describe the asynchronous data transfer in serial communications over synchronous data transfer. (CO1.5 — CL2)

**Asynchronous serial (definition & key points):**

* No shared clock line. Data transmitted with start bit(s) and stop bit(s) framing (e.g., start = 0, stop = 1). Receiver uses its own clock and detects start bit to sample data bits at expected bit intervals. Common in UART/RS-232. Advantages: simpler wiring, good for variable timing. Disadvantages: framing overhead (start/stop) and less efficient bit utilization.

**Synchronous serial (definition & key points):**

* Sender and receiver share a clock (separate line or embedded). Data transmitted continuously in blocks aligned to clock. Examples: SPI, I²C (SDA + SCL). Advantages: higher throughput and no per-byte framing overhead. Disadvantages: extra clock line or more complex synchronization.

**When to use:** Use asynchronous (UART) for simple long-distance/low-pin-count comms; use synchronous for higher throughput / tight timing between devices.

---

## 14. Explain the modes of Timer operation in the 8051 with diagrams. (CO1.5 — CL3)

**Overview (list & brief):**

* **Mode 0 (13-bit timer):** 13-bit timer — TL0 (8 bits) + 5 bits from TH0 — rarely used.
* **Mode 1 (16-bit timer):** Full 16-bit timer (TH/TL) — typical for delays.
* **Mode 2 (8-bit auto-reload):** TL auto-reloads from TH on overflow — useful for baud rate generation.
* **Mode 3 (split mode):** Timer0 split into two 8-bit timers (only for Timer0 on some 8051 variants).

**TMOD/TCON mapping (explain):**

* `TMOD` low nibble controls Timer0 mode bits M1,M0; high nibble Timer1.
* `TCON` contains TR0/TR1 (run control) and TF0/TF1 (overflow flags) and external interrupt edge bits.

**How to draw (diagram guidance):**

* Draw box for Timer0 with TH0/TL0 and label mode M1/M0 and TR0/TF0 bits in TCON. Indicate auto-reload path for Mode2 (TH0 → TL0 on overflow). For Mode1, show 16-bit concatenation. For Mode3 show TL0 and TH0 separate as timers.

**Usage note:** Mode2 is common for UART baud generation (with TH as reload), Mode1 for long delays using multiple overflows.

---

## 15. Analyze the effect of enabling multiple interrupts through the IE register on program execution. (CO1.6 — CL4)

**Explanation (bulleted):**

* **IE register** (`EA`, `ES`, `ET0`, `EX0`, `ET1`, `EX1`) enables/disables specific interrupts; `EA` is global enable.
* Enabling multiple interrupts allows the microcontroller to respond to various asynchronous events (serial, timers, external pins). When an enabled interrupt occurs, normal program flow is suspended and the CPU jumps to the corresponding interrupt vector; after ISR, execution resumes.
* **Effects & considerations:**

  * **Nested/Concurrent events:** If multiple interrupts occur, priority (via IP) and vectored timing determine which ISR runs first. Higher priority interrupts pre-empt lower ones only if priority bits and EA allow.
  * **Shared resources:** ISRs must protect shared data (disable relevant interrupts or use flags) to avoid race conditions.
  * **ISR duration:** Long ISRs increase latency for other interrupts and main program; keep ISRs short and use flags for longer processing in main loop.
  * **Stack use:** Frequent interrupts push return addresses onto stack; ensure sufficient stack space (SP management).
* **Conclusion:** Enabling multiple interrupts increases responsiveness but requires careful priority, reentrancy, and stack/resource management.

---

## 16. Draw and explain the interrupt structure/block diagram of the 8051. (CO1.6 — CL3)

**How to draw (on paper):**

* Draw a central CPU block with connections to five interrupt sources. Label interrupt sources and vectors.

**Interrupt sources & vector addresses (typical 8051):**

1. **External Interrupt 0 (INT0 / IE0)** — highest priority by vector position (address 0003H)
2. **Timer0 overflow (TF0)** — vector at 000BH
3. **External Interrupt 1 (INT1 / IE1)** — vector at 0013H
4. **Timer1 overflow (TF1)** — vector at 001BH
5. **Serial (RI/TI)** — vector at 0023H

**Control & priority blocks:**

* **IE (Interrupt Enable):** Enables individual interrupts (bits: EX0, ET0, EX1, ET1, ES) and EA (global enable).
* **IP (Interrupt Priority):** Assigns high(1)/low(0) priority to each interrupt; higher priority interrupts can pre-empt lower ones.
* **ISR flow:** On interrupt, CPU clears EA? (EA remains unchanged unless ISR modifies it), pushes PC to stack, vectors to fixed ISR address, executes ISR, then `RETI` returns and pops PC.

**ASCII mini-diagram (for exam):**

```
   [INT0] --\
   [T0 ] ----> [ Interrupt Controller ] --> CPU (vectored to 0x0003 / 0x000B / ...)
   [INT1] --/                                 |
   [T1 ] -------------------------------------|
   [Serial]-----------------------------------|
             IE (enable bits) & IP (priority)
```

**Explanation (brief):**
IE selects which interrupts are active; IP sets priority; when an enabled interrupt occurs the CPU jumps to the corresponding fixed vector, executes ISR, and returns. Priority resolves simultaneous interrupts; nested servicing follows IP settings.
