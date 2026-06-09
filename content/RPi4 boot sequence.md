---
title: RPi 4 Boot Sequence
---
1. [GPU] GPU wakes up. ROM bootloader baked into it --> `bootcode.bin`
2. [GPU] `bootcode.bin` --> `start.elf` (full GPU firmware). Loads peripherals, DRAM etc.
3. [GPU] `start.elf` --> `kernel8.img` in SD card. This transfers control to the SD card containing the boot image
4. [SD] Raspbian OS starts
- GPU is the primary processor in RPi 4. Hardware decision by Broadcom, I don't understand why. Usually, CPU is the primary processor
- Multi-stage bootloader because ROM bootloader is tiny and cannot do everything

Linker Script for RPi4
- `.text` - Stores machine code instructions
	- `.text.boot` - Boot code. *Goes first before* `.text` because you don't want random code to run BEFORE booting
- `.data` - Stores initialized global/static variables
- `.bss` - Stores uninitialized global/static variables
- `.rodata` - Read-only data. Stored to DRAM in RPi4 (marked read-only by convention)
- `*` - Glob operator. For example, `*(.text)` - "Gather `.text` from *every* object file and place it here"
- `. = 0x80000` - Start address for booting in DRAM. This is a DRAM physical address, not that of GPU

- In ARM, stack memory goes downward while code memory goes upwards. Stack and code never meet until something is catastrophically wrong. For example, `mov sp 0x80000` loads the start address of `0x80000` into the `s`tack `p`ointer. As functions are called, the stack pointer goes downwards towards lower addresses

boot.S script
```asm
.section ".text.boot"
.global _start

_start:
	mrs x0, mpidr_el1
	and x0, x0, #0xFF
	cbnz x0, halt
	
	ldr x1, =0x80000
	mov sp, x1
	
	bl main
	
halt:
	wfe
	b halt
```
Explanation:
`.section` says "`.text.boot` is the section this assembly code is going into", and in that, we define `_start` as the entry point (much like how `main` is the entry point in a C program). When running `_start`, the following happens in sequence:
1. The CPU core ID of core 0 (first core) is read into register `x0` using `MRS` (Move Register from System Register). `mpidr_el1` stands for [MultiProcessor Affinity Register (Exception Level 1)](https://developer.arm.com/documentation/ddi0601/2025-12/AArch64-Registers/MPIDR-EL1--Multiprocessor-Affinity-Register) and it contains info about CPU cores. ==`mov` can't work because this is a special system register for which `mrs` is used==
2. Extract the lowest byte, which alone contains the core's ID. Use `and` for that
3. `cbnz` - Change Branch if Not Zero. If we don't read core 0, `halt` the program (stop it). Everything should fire from core 0 basically
4. Load address `0x80000` (start address in DRAM) to register `x1` and then move that address to the stack pointer. `sp` tracks function calls here and address jumps, that's why. *There's technically no stack in DRAM but program loading ==requires== stack operations mandatorily*
5. Then, we move to `main` (that's defined by the object we create that is linked later using `linker.ld` and `boot.S`). `bl` is the register that means "Branch Load". When `main` returns, focus moves back to the exact part from where `main` was called
6. Once `main` returned, `wfe` (Wait For Event) and branch to `halt`. This stops execution. There's no OS underneath considering this is bare-metal, so it just idles because it's a loop (go to branch `halt`, `wfe`, and then go to branch `halt` and so on)
- [ ] #q Why can't we do `mov sp #0x80000` directly? Why split into two commands?
- [/] #q I did not understand the ARM documentation for MPIDR_EL1. 
	- [x] What is this for?
		- [Used for processor identification in a multi/uniprocessor system, for scheduling](https://developer.arm.com/documentation/ddi0406/b/System-Level-Architecture/Virtual-Memory-System-Architecture--VMSA-/CP15-registers-for-a-VMSA-implementation/c0--Multiprocessor-Affinity-Register--MPIDR-?lang=en#:~:text=The%20Multiprocessor%20Affinity%20Register%2C%20MPIDR,32%2Dbit%20read%2Donly%20register)
	- [/] Where is the core ID stored? Claude says lowest byte but I see "affinity levels" there for PEs
		- By the way, PE = Processing Element
