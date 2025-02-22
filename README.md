![](../../workflows/wokwi/badge.svg) 
Go to https://tinytapeout.com for instructions! 

# MicroAsicVI - Oscillators & TRNG

This is TinyTapeout submission on Verilog. It implements 7 oscillators, and divides clock to allow external measurements via slow IO. 
As we only have access to Verilog, to ensure oscillators are not optimized away they use shift register data as second input. With physical chips at hand one will be able to compare practical results vs analog simulation at multiple temperatures / voltages.

True random number generator is XOR of output of multiple ring oscillators, latched by clk_in.  

# In pinout: 
```
0: clock in (for debugging)
1: reset
2: shift register clk
3: shift register data
4-6: clock source id
7: unused
```

# Out pinout: 
```
0: clock divided by 2^10
1: clock divided by 2^14
2: clock divided by 2^18
3: clock divided by 2^22
4: clock divided by 2^26
5: clock divided by 2^30
6: True random number generator output (generator type selected by 3-bit clock selection)
7: Bit 11 of shift register (to ensure it's not optimized away)
```

# Oscillator selection:
XOR, NAND requires ones in shift register. 
NOR requires 0's. 
Full adder requies odd or even bits set to 1 (but not both). 

```
b000: clk_in
b001: 3-stage XOR oscillator (can be set to 1 or 3 inversions through shift register)
b002: 5-stage XOR oscillator (can be set to 1, 3 or 5 inversions through shift register)
b003: 1-stage XOR oscillator (unlikely to work)
b004: 2-stage XOR oscillator (requires only one '1' in shift register to have single inversion)
b005: 5-stage NAND oscillator (can be set to 1, 3 or 5 inversions through shift register)
b006: 5-stage NOR oscillator (can be set to 1, 3 or 5 inversions through shift register)
b007: 5-stage Full adder oscillator
```


