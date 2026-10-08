# ECE_528_HW2

## Section I: Review Questions


**1.	(5 pts) What is a hardware timer? What kinds of tasks do embedded systems commonly use hardware timers for?**

Hardware timers are dedicated counters in an embedded system that use clock signals to perform their counting. These timers are commonly used to generate event triggers such as interrupts, to generate pulse-width modulation (PWM) signals, and for anything else that can use a clock signal.


**2.	(5 pts) What is a clock signal used for in embedded systems? List the primary clock sources available on the MSP432P401R microcontroller and briefly describe each clock source.**

A clock signal is used to coordinate events within the embedded system itself, as well as with external modules that use the clock signal as a reference point. The microcontroller has the following clock signals:

1. ***ACLK:*** Auxiliary clock
This is the slow, low-power clock, topping out at 128 kHz. Use it when there is a need to keep time without burning much power.

1. ***MCLK:*** Master clock
This is the main clock that runs the CPU. It can come from any of the clock sources and goes up to 48 MHz.

1. ***HSMCLK:*** Subsystem master clock
This is the fast clock for peripherals that need speed.

1. ***SMCLK:*** Low-speed subsystem master clock
This is the slower version of HSMCLK. It uses the same source as HSMCLK but has its own divider, and it maxes out at half of HSMCLK's top speed.

1. ***BCLK:*** Low-speed backup domain clock
This is the clock for the backup domain, mostly the real-time clock (RTC).


The following 7 clock sources are available on the MSP432P401R microcontroller:

1. ***LFXTCLK:*** Low-frequency oscillator (LFXT) that can be used with either low-frequency crystals or external clock sources. It can be driven with an external square-wave signal in the 32-kHz or below range.

1. ***HFXTCLK:*** High-frequency oscillator (HFXT) that can be used with standard crystals in the 1-MHz to 48-MHz range. It can also be driven with an external square-wave signal.

1. ***DCOCLK:*** Internal digitally controlled oscillator (DCO) with programmable frequencies and a default frequency of 3 MHz.

1. ***VLOCLK:*** Internal very-low-power, low-frequency oscillator (VLO) with a typical frequency of 9.4 kHz.

1. ***REFOCLK:*** Internal low-power, low-frequency oscillator (REFO) with selectable typical frequencies of 32.768 kHz or 128 kHz.

1. ***MODCLK:*** Internal low-power oscillator with a typical frequency of 25 MHz.

1. ***SYSOSC:*** Internal oscillator with a typical frequency of 5 MHz.


**3.	(5 pts) Consider a 16-bit timer (e.g. Timer_A configured in up mode). If it counts upward, what is the maximum count value before it rolls over? Show your work.**

For a 16-bit timer, we use the following equation to get the maximum value:

Total number of values $ = (2^n-1) \rightarrow (2^{16}-1) = 65535 $

We subtract 1 because the count starts at 0.


**4.	(5 pts) If a 24-bit timer counts upward from zero at 10 MHz, what is its overflow period? Show your work.**

The overflow period is the amount of time it takes for the timer to count through its entire range of values. It can be calculated as follows:

Total number of values $ = (2^n-1) \rightarrow (2^{24}-1) = 16777215 $

Time for one count = $\frac{1}{f} = \frac{1}{10,000,000 Hz} = 0.0000001s = 100ns $

Total Time = $ 16777215 * 100ns = 1.677s $


**5.	(5 pts) Describe the SysTick timer on the MSP432P401R microcontroller. Provide two example applications for SysTick in embedded systems. Which clock sources can the SysTick timer use and how can they be selected?**

SysTick is "a simple, 24-bit clear-on-write, decrementing, wrap-on-zero counter with a flexible control mechanism."

It can be used as a high-speed alarm timer driven by the system clock, or as a simple counter to measure elapsed time and time to completion.

According to Table 2-54 of the MSP432P401R Technical Reference Manual, the SysTick timer uses the "Core clock," which is presumably the master clock (MCLK), the clock used by the CPU.


**6.	(5 pts) How many registers does the SysTick timer have on the MSP432P401R microcontroller and what is the purpose of each? Name them and provide a short description per register.**

SysTick has 3 registers: the SysTick Control and Status Register (STCSR), the SysTick Reload Value Register (STRVR), and the SysTick Current Value Register (STCVR).

***STCSR Register:*** Configures the clock source, enables the counter, reports the counter status, and enables the timer interrupt.

***STRVR Register:*** Holds the reload value for the counter, which sets the value the counter wraps around to.

***STCVR Register:*** Contains the current value of the counter.


**7.	(5 pts) Describe the Timer_A peripheral on the MSP432P401R microcontroller. List the main operating modes of Timer_A and briefly explain each.**

Timer_A is a 16-bit timer/counter with 7 capture/compare registers. It has a variety of interrupt options, which can be generated on overflow conditions or by the capture/compare registers.

It has 4 main modes: stop, up, continuous, and up/down.

1. ***Stop:*** The timer does not count at all.

1. ***Up:*** The timer counts up to a specific value given to the compare register. Once that value is reached, the count resets back to 0.

1. ***Continuous:*** The timer counts up to the maximum value of the register; only then does it reset back to 0.

1. ***Up/Down:*** The timer counts up to a specific value given to the compare register. Once that value is reached, the timer begins counting back down.


**8.	(10 pts) Briefly state the purpose of the following Timer_A registers.**

1. ***TAxCTL:*** The Control Register is used to configure the timer, including the clock source, input divider, mode select, and interrupt enable.

1. ***TAxCCTLn:*** The Capture/Compare Control Register is used to configure the capture/compare functionality of the timer. It controls the capture mode type (rising edge, falling edge, both, or none), the input select pin, synchronized capture (sync or async), the mode type (capture or compare), the output mode (a combination of set, reset, and toggle), capture overflow (yes or no), and the capture/compare interrupt enable.

1. ***TAxR:*** The Counter Register contains the actual count of the timer.

1. ***TAxCCRn:*** The Capture/Compare Register holds the value that is compared to the timer count, or captures (copies) the counter value into the register.

1. ***TAxIV:*** The Interrupt Vector Register contains the interrupt sources for all of the capture/compare registers. Depending on which register triggered the interrupt, it has a certain priority level.

1. ***TAxEX0:*** The Expansion Register contains additional divider values used as a prescaler to set a certain clock speed.


**9.	(5 pts) What is PWM? Provide at least three example applications that use PWM in embedded systems.**

PWM, or pulse-width modulation, is a signal that is "High" for a set duration and "Low" for a set duration. The ratio of High to Low is known as the duty cycle, and together they make up one period (cycle) of the signal.

Three examples of where PWM signals are used in embedded systems are 1) motor control, 2) servo position control, and 3) LED brightness. In short, PWM is a way to take the discrete values 0 and 1 and, depending on the duty cycle, create an averaged control signal between 0 and 1 (0% to 100%).


**10.	(5 pts) How many Timer_A modules are available and how many PWM outputs can each module provide?**

There are 4 Timer_A modules that can be used for PWM (TA0,TA1,T2,T3) with five capture/compare registers each. Which means that if we have capture/compare registers 0 be out refrence we have 4 other register to be use to generate 4 PWM sigal. Which means intotal we can get 16 PWM singls form alll the Timer A modules combined. 




## Section II: Code

**11.	(25 pts) Describe the steps needed to initialize P2.4 as a PWM output driven by Timer_A0 CCR1 (TA0.1).**

Use the Timer_A2_PWM.c driver provided in Lab 1 (Motor Control) as a reference.

Include C code such that:

***1. The P2.4 pin is configured to use its primary module function (Timer_A output)***

Table 12-2 of the Technical Reference Manual and Table 6-67 of the datasheet show that P2.4 is set to its primary function (Timer_A output) with the following bit 4 settings:

| Register | Bit 4 |
|---|---|
| `P2SEL1` | 1 |
| `P2SEL0` | 0 |
| `P2DIR` | 1 (output) |


***2. SMCLK is selected as the clock source for Timer_A***

To select SMCLK, we can refer to Table 19-4 of the Technical Reference Manual, which describes the `TAxCTL` register. Of the 16 bits in this register, we focus on bits 9–8, which make up the `TASSEL` field. SMCLK is selected by setting bit 9 to 1 and bit 8 to 0.

From the table: `10b = SMCLK`


***3. The timer clock is divided by 2***

Table 19-4 also shows that bits 7–6 of `TAxCTL` (the `ID` field) control the timer input divider. To divide by 2, we set bit 7 to 0 and bit 6 to 1.

From the table: `01b = /2`


***4. Up/down mode is selected***

Table 19-4 also shows that bits 5–4 of `TAxCTL` (the `MC` field) control the timer mode. To select up/down mode, we set both bit 5 and bit 4 to 1.

From the table: `11b = Up/down mode`


***5. The output mode is configured as Toggle/Reset by setting the appropriate bits of the OUTMOD field in the CCTL[1] register***

Table 19-6 of the Technical Reference Manual describes the `TAxCCTLn` register. Bits 7–5 make up the `OUTMOD` field, which sets the output mode. To select Toggle/Reset, we set bits 7–5 to `010b`.

From the table: `010b = Toggle/reset`


***6. The PWM signal has a period of 20 ms and a duty cycle of 50%***

**Period:** We are given a period of 20 ms, so we calculate the period constant using the following general formula:

$$ \text{Period} = \frac{2 \times \text{Period constant}}{12\text{ MHz} \,/\, \text{Prescale value}} $$

Solving for the period constant:

$$ \text{Period constant} = \frac{\text{Period} \times (12\text{ MHz} \,/\, \text{Prescale value})}{2} $$

Plugging in our values:

$$ \frac{0.020\text{ s} \times (12\text{ MHz}/2)}{2} = \frac{0.020\text{ s} \times 6{,}000{,}000\text{ Hz}}{2} = \frac{120{,}000}{2} = 60{,}000 $$

So `TIMER_A0->CCR[0] = 60,000`.

**Duty cycle:**

$$ \text{Duty cycle} = \frac{\text{Input duty value}}{\text{Period constant}} $$

$$ \text{Input duty value} = \text{Duty cycle} \times \text{Period constant} = 0.50 \times 60{,}000 = 30{,}000 $$

So `TIMER_A0->CCR[1] = 30,000`.

**Check:**

$$ \text{High time} = \frac{2 \times 30{,}000}{12\text{ MHz}/2} = \frac{60{,}000}{6{,}000{,}000\text{ Hz}} = 10\text{ ms} $$

This is 50% of the 20 ms period.


***Code Implmentation***

```c

// Setting up Port 2 Pin 4 (2.4) 
    P2->SEL0 |= 0x10;  \\ Setting bit 4 to value of 1 for SEL0 
    P2->SEL1 &= ~0x10; \\ Clearing bit 4 to value of 0 for SEL1 
    P2->DIR |= 0x10;   \\ Setting bit 4 to value of 1 for SEL0

// Setting Up CTL register for Timer A 
    // Select SMCLK = 12 MHz as timer clock source -> From the table: `10b = SMCLK` for bits 9-8
    // Set ID = 1 (Divide timer clock by 2) -> From the table: `01b = /2` for bits 7-6
    // Set MC = 3 (Up/Down Mode)-> From the table: `11b = Up/down mode` for bits 5-4
    // Results in a Bit Mask of = 0000 0010 0111 0000 --> Hex Equivalent of 0x0270

    TIMER_A2->CTL |= 0x0270;

// Setting up CCTL[1] "Output Capture" mode to Toggle/Reset

                                  // Setting Toggle/Reset -> From the table: `010b = Toggle/reset` for bits 7-5
    TIMER_A2->CCTL[1] |=0x0040    // Bit Mask = 0000 0000 0100 0000 --> Hex Equivalent of 0x0040

// Setting as a Duty Cycle of 50%
    TIMER_A2->CCR[1] =300000 ;     // Value calculated above 


```


**12.	(20 pts) Describe the steps needed to configure Timer_A0 to generate a periodic interrupt every 2 ms with an interrupt priority level of 2. Note that Timer_A0 has an interrupt number of 8.**

Use the Timer_A0_Interrupt.c driver provided in Lab 1 (Motor Control) as a reference.

Include C code such that:

***1. SMCLK is selected as the clock source for Timer_A with a prescaler value of 1 (which means the ID field will be 00b)***

As in Question 11, Table 19-4 of the Technical Reference Manual shows that bits 9–8 of `TAxCTL` (the `TASSEL` field) select the clock source. SMCLK is selected by setting bit 9 to 1 and bit 8 to 0.

From the table: `10b = SMCLK`

The same table shows that bits 7–6 (the `ID` field) control the input divider. To divide by 1, we set both bit 7 and bit 6 to 0.

From the table: `00b = /1`


***2. The TAIDEX field in the TAxEX0 register has a value of 000b (divide by 1)***

Table 19-9 of the Technical Reference Manual describes the `TAxEX0` register and confirms that a `TAIDEX` value of `000b` divides the input clock by 1, matching the given requirement.

From the table: `000b = Divide by 1`


***3. Up mode is selected***

Table 19-4 of the Technical Reference Manual shows that bits 5–4 of `TAxCTL` (the `MC` field) control the timer mode. To select up mode, we set bit 5 to 0 and bit 4 to 1.

From the table: `01b = Up mode`

***Code Implmentation***

```c

    // In the CTL register, set the TASSEL and ID bits
    // Choose SMCLK as timer clock source (TASSEL = 10b)
    // Choose prescale value of 1 (ID = 0)
    TIMER_A0->CTL |= 0x0200;

//Left off here 

```
