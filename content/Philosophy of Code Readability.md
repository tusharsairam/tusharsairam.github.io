---
title: Code Readability
---
Programming is a large part of both my job and my personal projects. I put a lot of emphasis on code readability because I stare at code frequently. Over the course of my experienced and taking into consideration what many people say, here is a collection of thoughts and guidelines that I tend to follow:

- Write code so that **it's easy for someone else unfamiliar with it to understand it**[^1]
- Your code should make it **easy for future you to come back to it and understand what it does**[^1]
- Code should **not be read top-to-bottom** like articles (unless you're auditing code). **Read them by tracing the control flow**[^1]
- It's **impossible to write perfect and bug-free code**. Focus on **writing code such that bugs can be easily traced and fixed** when they happen[^1]
- Always use **8-space/tab indentation** over 4-space/tab indentation. Linus Torvalds outlines the benefits well[^2]
	- Easier to read because the indentations are more pronounced
	- Free warning when you have too many nested loops because your code shoots off faster towards the right edge of your screen!
- **Identifier names should be short and descriptive**. `atom_counter` conveys its purpose better than `foo`, for instance
- **Functions should have their opening brace below their definition line** while **conditionals should have their opening brace in the same line**[^3]
- **Comments should be adequately descriptive**. Use them wisely
	- Comments should describe what the code itself cannot describe easily
	- Functions should have an accompanying doxygen comment (C/C++)/dosctring (Python)
- Every function should **have one defined role to play**. If it's doing a lot of stuff, split it unless it's impossible or nonsensical to do so
- In production code, **leave clever optimizations and tricks to the compiler**. Exceptions exist as always though! 

[^1]: [YouTube: Internet of Bugs -- Clean Code is Bad. How to write maintainable code?](https://www.youtube.com/watch?v=8ncQrGuunHY)
[^2]: [Linux Kernel Coding Style Guidelines - Indentation](https://www.kernel.org/doc/html/v4.10/process/coding-style.html#indentation)
[^3]: [Linux Kernel Coding Style Guidelines - Braces and Spaces](https://www.kernel.org/doc/html/v4.10/process/coding-style.html#placing-braces-and-spaces)
