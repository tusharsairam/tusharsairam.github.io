---
date: 2026-03-15T11:58:00
---
**LookUp Table (LUT)**: FPGA component that maps inputs to an output. The *output of a LUT is configurable by the developer* 

LUTs are characterized by the no. of inputs to it and their **truth table**. For instance, a 3-input LUT looks like this

| A   | B   | C   | Out | Index     |
| --- | --- | --- | --- | --------- |
| 0   | 0   | 0   | 0   | `SRAM[0]` |
| 0   | 0   | 1   | 1   | `SRAM[1]` |
| 0   | 1   | 0   | 0   | `SRAM[2]` |
| 0   | 1   | 1   | 1   | `SRAM[3]` |
| 1   | 0   | 0   | 0   | `SRAM[4]` |
| 1   | 0   | 1   | 1   | `SRAM[5]` |
| 1   | 1   | 0   | 0   | `SRAM[6]` |
| 1   | 1   | 1   | 1   | `SRAM[7]` |

`Out` is configurable, and the value of `Out` is stored in the LUT's SRAM blocks. The memory structure looks like this: `SRAM[Inputs Index] = Output`. The inputs `A`, `B`, `C` in this case are connected to a 8:1 mux (which physically maps `LUT[A][B][C]` --> `LUT[0])`

>[!note] MUX input count
>If $X$ is the no. of inputs to the LUT, the no. of MUX inputs are $2^X$ (binary values), so in this example, we have 3 inputs to the LUT, so the mux is $2^3:1$ = $8:1$

LUTs are powerful because you can represent arbitrary complex math functions using combinatorial logic, which is *faster than running an algorithm to calculate the output*.
