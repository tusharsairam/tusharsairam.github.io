---
date: 2026-01-03T17:29:00
---
- UEFI is entirely controlled by Microsoft actually. They wrote the UEFI specifications and it expects x86-64 PE32+ (really just the `.exe` Executable). You *could* use GNU-EFI to output such an executable while working in Linux using the standard GNU-GCC toolchain but that would be complex. Still trying to figure it out
- Turns out LLVM/Clang supports building EFI binaries and is easier than using GNU-EFI (I was earlier following the [OS-Dev Wiki entry on GNU-EFI](https://wiki.osdev.org/GNU-EFI) to learn how to write EFI binaries but the entry is badly written -- It's not clear as to whether I should use the pre-installed EFI libraries that come with the system... they don't, or whether I should use the headers spit out by compiling the contents of the GNU-EFI repo. Second, I ran their minimal Hello World example and I got a ton of errors)
- When compiling the code, some flags must be issued to the compiler
	- `-ffreestanding` - The program is run in a location where access to common libraries isn't guaranteed (it's not a hosted environment), so we prime the program to run in a free-standing environment 
	- `-mno-red-zone` - 

# Sources
- https://dvdhrm.github.io/2019/01/31/goodbye-gnuefi/
- https://krinkinmu.github.io/2020/10/11/efi-getting-started.html