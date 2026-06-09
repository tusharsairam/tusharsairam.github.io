---
title: Busy Beaver Functions
created: 2025-08-03 23:08:53
modified: 2025-08-03 23:08:53
---
>[!definition] Busy Beaver
>An n-state **Turing Machine** which writes a maximum number of 1s ($\Sigma(n)$) before **halting**.[^1]

Another definition of a busy beaver is that it's a ==game whose objective is to **find a terminating program** of a given size that either produces the most output possible or runs for the longest number of steps==[^2]. "Most output" can be thought of as writing as many 1s to the tape as possible before stopping

$BB(n)$ means *busy beaver function given an n-state Turing machine, starting with an all-0 tape*. It is also called **Rado's sigma function**

My momentary interest in busy beavers and theoretical computer science sparked upon reading [this blog entry by Scott Aaronson](https://scottaaronson.blog/?p=8972). Busy Beaver numbers $BB(n)$ start out pretty small. The first four numbers are already known
$$
BB(n)_{n = 1,2,3,4}= 1, 4, 6, 13
$$
However, $BB(5)$ and beyond? Those numbers are huge. So huge that only lower limits for those numbers could be specified. $BB(5) \geq 4098$ and $BB(6) \gt 2 \uparrow\uparrow 2 \uparrow\uparrow 2 \uparrow\uparrow 9$! That's an INSANELY HUGE number!

>[!note] What does $\uparrow\uparrow$ mean?
>Knuth's up-arrow notation $\uparrow$ when repeated to make two such arrows represents the **tetration** operator. Tetration is **repeated exponentiation**

[^1]: [Wolfram MathWorld: Busy Beavers](https://mathworld.wolfram.com/BusyBeaver.html)
[^2]: [Wikipedia: Busy Beaver](https://en.wikipedia.org/wiki/Busy_beaver)
