## **Answer**

### **Introduction**

An **embedded system** is a small computer built inside hardware devices to perform specific tasks.
One of the simplest tasks is **blinking an LED** using a microcontroller. This demonstrates how to control hardware pins with C code by turning them **ON and OFF** with a fixed delay.

---

### **Main Explanation**

To blink an LED, we need three things:

1. **Hardware setup**

   * Connect an LED to one of the output pins of the microcontroller (e.g., **P1.0** of 8051).
   * Use a **current-limiting resistor (≈330Ω)** in series with the LED.

2. **Software logic**

   * Make the pin **HIGH (1)** → LED turns ON.
   * Wait for some time (delay).
   * Make the pin **LOW (0)** → LED turns OFF.
   * Wait for some time.
   * Repeat the above steps forever (loop).

3. **Delay creation**

   * In C, delays can be created using a **for loop** or using a **timer**.
   * Here we use a simple **for loop delay** for demonstration.

---

### **Diagram (ASCII sketch)**

```
     +5V
      |
     [330Ω]
      |
     LED
      |
   P1.0 pin of 8051
      |
     GND
```

---

### **Embedded C Code (for 8051, Keil C style)**

```c
#include <REG51.H>   // Header file for 8051 registers

// Simple delay function
void delay(unsigned int time) {
    unsigned int i, j;
    for(i = 0; i < time; i++) {
        for(j = 0; j < 1275; j++); // Creates ~1ms delay
    }
}

void main(void) {
    while(1) {
        P1 = 0x01;   // Set P1.0 = 1 → LED ON
        delay(500);  // Delay ~500 ms

        P1 = 0x00;   // Set P1.0 = 0 → LED OFF
        delay(500);  // Delay ~500 ms
    }
}
```


## Code Explanation :

## **Step-by-step Explanation**

```c
#include <REG51.H>   // Header file for 8051 registers
```

* `#include` means "add a library/header file."
* `REG51.H` is a file that tells the compiler about **special names** for the 8051 microcontroller.

  * Example: It tells the compiler that **P1** means "Port 1 register".
* Without this line, the compiler wouldn’t know what `P1` is.

---

```c
// Simple delay function
```

* Anything after `//` is a **comment** → written only for humans, not executed.
* This is just a note saying "we are making a delay function."

---

```c
void delay(unsigned int time) {
```

* This defines a **function** named `delay`.
* `void` means the function does not return any value.
* `unsigned int time` means the function will accept a number (positive integer) called `time`.

  * Example: if we call `delay(500)`, then inside the function `time = 500`.

---

```c
    unsigned int i, j;
```

* Inside the function, we are creating two variables (`i` and `j`).
* Both are `unsigned int` → positive numbers.
* These will be used in loops to "waste time" (make a delay).

---

```c
    for(i = 0; i < time; i++) {
```

* This is a **for loop**.
* Start: `i = 0`.
* Check: if `i < time`. If true, run the code inside.
* After each round, `i++` increases `i` by 1.
* This will repeat `time` number of times.

  * If `time = 500`, it will repeat 500 times.

---

```c
        for(j = 0; j < 1275; j++);
```

* Another loop inside the first one.
* `j` goes from `0` up to `1274`.
* `;` means there is **no code inside** → it’s just wasting processor time.
* This is how we create a **time delay** in software.
* Together, the two loops create \~1 millisecond per count of `time`.

---

```c
}
```

* This closes the `delay` function.

---

```c
void main(void) {
```

* This is the **main function**.
* Every C program **starts executing from `main()`**.
* `void` means it does not return anything.

---

```c
    while(1) {
```

* `while(1)` means "do forever."
* Because `1` is always true, the loop never ends.
* This makes the LED keep blinking **continuously**.

---

```c
        P1 = 0x01;   // Set P1.0 = 1 → LED ON
```

* `P1` means **Port 1 register** (8 pins P1.0 to P1.7).
* `0x01` is **hexadecimal** for `0000 0001` in binary.

  * This means **P1.0 = 1** (HIGH), and all other pins are 0.
* If LED is connected to P1.0, it turns **ON**.

---

```c
        delay(500);  // Delay ~500 ms
```

* Calls the delay function with `time = 500`.
* This keeps the LED ON for about 500 milliseconds.

---

```c
        P1 = 0x00;   // Set P1.0 = 0 → LED OFF
```

* Now we write `0x00` (binary `0000 0000`) to Port 1.
* This means all pins (including P1.0) = 0 (LOW).
* So, the LED turns **OFF**.

---

```c
        delay(500);  // Delay ~500 ms
```

* Again, call the delay function.
* Keeps the LED OFF for 500 milliseconds.

---

```c
    }
}
```

* These close the while loop and main function.
* Because `while(1)` is infinite, the LED keeps **turning ON, delay, OFF, delay… forever.**

---

## **How the program actually runs (inside microcontroller)**

1. Start program → go into `main()`.
2. `while(1)` → infinite loop begins.
3. Turn LED ON (P1.0 = 1).
4. Delay for \~0.5 second.
5. Turn LED OFF (P1.0 = 0).
6. Delay again.
7. Repeat forever → LED blinks.
