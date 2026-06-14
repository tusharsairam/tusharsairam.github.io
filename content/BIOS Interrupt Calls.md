---
title: BIOS Interrupt Calls
---
BIOS interrupt calls are software interrupts that an OS or a bootloader uses to request specific services/processes from the BIOS. A quick example of that in x86 Real Mode architecture is `INT 19h` which sends an interrupt to the BIOS telling it to reboot the computer

Here's a breakdown of the interrupt instruction:
1. `INT`: This means the instruction is a software interrupt instruction
2. `19h` is the interrupt vector number. The vector number identifies the interrupt function

The processor takes this interrupt instruction and queries an Interrupt Vector Table (IVT), which is more like a LUT that contains the addresses of all the ISRs (Interrupt Service Routines) that are mapped to their corresponding vector numbers. The address of the entry (the address of the ISR) is calculated as follows:
$$
\text{ISR}_{\text{Entry}} = \text{IVT}_{\text{base}} + (\text{Vector Number} \times \text{Entry Size})
$$
The IVT base in x86 systems (by the way, the correct term for x86 interrupt tables is an IDT or Interrupt Descriptor Table) is 0x0000. Think of this as the address that locates the start of the IVT. The vector number is, in this example, `19h` (what follows the `INT` instruction) and each entry in the IVT has a size (in bytes) that is $\text{Entry Size}$. In Real Mode, each entry's 4 bytes, for instance. The best way to quickly and intuitively understand this is -- To execute a desired interrupt, you do these 
1. You want to execute a software interrupt (that's `INT`)
2. What interrupt do you want to execute? That's defined by the next instruction (here, it's `19h`)
3. The computer stores a LUT that says "If you give me the vector number, I'll look it up and give you the interrupt handler for it". That LUT is the IVT
4. The interrupt handler starts the desired interrupt

>[!note] Modern PCs don't use BIOS interrupt calls beyond the booting phase
>This is because most modern OSes can communicate with devices through drivers easily, so there's no necessity for the BIOS as a "middleman". Also, modern kernels run in 32-bit Protected Mode but BIOS interrupts function in the older Real Mode. A computer would have to switch from Protected to Real Mode, handle the interrupt, and switch back, which is tedious

What I find really interesting about this is not merely the historical operation of computers -- This reveals a more general pattern I see in many other computer architectures as well:
- STM32 microcontrollers also store IVTs (they're just referred to as vector tables) and any interrupts (be it software or external interrupts from hardware called EXTIs) are prioritized and managed smoothly by the Nested Vectored Interrupt Controller (NVIC). AVR microcontrollers also have vector tables baked in. Linux uses an IDT that is stored in the IDT register. Looks like most every computer architecture/kernel uses an IVT/IDT and the overall structure and mechanism of access is the same
- Interrupt functions are known from the get-go, so a LUT is created to ensure fast deterministic execution of events. The model is that LUTs are defined for when we know and can map one quantity to another. LUT query is fast, because they're usually hashed, so it only makes sense that computer interrupts are built in a LUT -- Context switching should be pretty fast and most interrupts are system-critical

>[!info] Why are interrupt vectors called so?
>I imagine they're called so because an IVT is like a 1D vector, with each element being the ISR mapped to each interrupt vector, that's akin to the element's index. However, a better name is IDT (which x86 and Linux kernel uses)

Some good resources I can quickly consult to dive into the details:
- https://wiki.osdev.org/Interrupt_Vector_Table
