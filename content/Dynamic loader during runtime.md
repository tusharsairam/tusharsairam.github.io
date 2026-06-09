---
date: 2026-03-15T02:16:00
---
`ld` - Loader (also called the Linker)

This is called for **dynamically linked ELF executables**

The loader maps dependencies first (checks if `libc` is required, and where if so etc.) and then resolves the symbols

You can use `__attribute__` in C to put information into specific sections. For example, `__attribute((section('.interp'))` injects the supplied string into the `.interp` ELF section. The string is the location to the linker (`/lib/ld-linux.so.2` for instance)

