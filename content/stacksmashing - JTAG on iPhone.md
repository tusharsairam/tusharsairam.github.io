---
title: Notes on stacksmashing's talk on Apple ReverseEngg
---

>[!info] Objective
>Thomas Roth a.k.a stacksmashing gave a talk at DEF CON about being able to ==get JTAG on the iPhone 15== and ==dumping Apple's USB-C controller==. Here are my notes from his [talk](https://www.youtube.com/watch?v=cFW0sYSo7ZM)

![[USB-C Pinout.png]]

Recent Apple devices come with a **USB-C PD Controller** that performs *PD negotations* with a cable that is inserted. The PD negotiations happen through designated pins of the USB-C port. The pins of a USB-C port and plug look like this. You can use **VDMs (Vendor-Defined Messages)** to *customize what some pins can do* -> allows JTAG on iPhone and opening UART shell. Specifically, *CC1* and *CC2* are where the negotiations happen unless changed with VDMs
 
Apple uses the *ACE2 microcontroller* (just a TI chip) that bridges the USB port and the system's SoC. Communications happen using the **AppleHPMBus protocol (Host Port Microcontroller)**. This allows sending 4-byte integer/ASCII commands called **FourCC commands**, which can be used to dump firmware. Some privileged commands exist (similar to PEEK and POKE) but locked on production devices

However, the SPI Flash in the ACE2 chip does not contain the full firmware, only patches. Makes debugging annoying. Updates to the device are protected with RSA3072 but that is checked only during updates -> ==no secure boot==. Get around this and you get persistence for free 

SoC --> SWD --> ACE2
SoC <-- SWD <-- ACE2

- Use GPIO pins to debug the ACE2 through SWD 
- Debug pathway: Userspace application -> IOKernelRW -> Memory-mapped GPIO peripheral -> Physical pins -> ACE2
- ACE2 backdoorable by disabling signature verification 
- Successor to ACE2: ACE3 -> Runs a full USB stack, TI chip underneath had no public documentation 
- Possible to directly debug and dump ACE3 firmware because it has a full USB stack (port DFU)
- Updates are fully personalized (each unique chip has its own updates) + patches exist, not the full firmware
- Unable to get firmware (tried software vulnerability, physical SWD access (debug connector disabled), modify Flash, Fuzzing etc)
- Alternative: **Fault Injection** -- Insert faults on purpose to modify chip behaviour
- Fault injection technique used - EMFI (*precise timing information needed* -> Side channel)
- Record EMI emissions after invalidating Flash -> Check alignment of recordings -> Check CRC correctness (this I'm not sure how it works here)
