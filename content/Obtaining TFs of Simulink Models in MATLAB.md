---
title: Obtain TFs from Simulink model
---

```matlab
sys = linearize(mdl) # Simulink model name without the ".slx" extension name

[num, den] = tfdata(sys, 'v')

G = tf(num, den)
```

`G` is your transfer function

`tfdata` will only work on LTI systems. Non-linear models will require either alternative approachs or linearization about an operating point using `linearize()`. I've found this approach helpful when I had to provide the transfer function of a reaction wheel model on Simulink. By the way, the `'v'` argument there instructs MATLAB to output the numerator (`num`) and denominator (`den`) coefficents as row vectors instead of cell arrays

You can get multiple TFs from this command. To pick the transfer function of interest, specify `G(out, in)` where `out` and `in` represent the indices of the arrays containing the output and input lines. For example

```
1 --- [ TF1 ] - 2 - [ TF3 ] --- 4
3 --- [ TF2 ] --- 5 
```

To view the transfer function of `TF3`, issue `G(4, 2)` to the command line
