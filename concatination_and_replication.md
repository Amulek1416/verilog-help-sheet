[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Concatination and Replication

## Concatination
Concatination is used to group togther multiple variables into a bus, multiple busses into a single bus, or multiple parts of busses into a single bus.
 
- Example, concatinating multiple 1-bit wires:
>```verilog
>wire [3:0] a;
>assign a = {btnA, btnB, btnC, btnD};
>// The MSB of `a` becomes equal to btnA,
>// while the LSB of `a` becomes equal to btnD.
>```
 
- Example, concatinating two busses together:
>```verilog
>wire [3:0] a, b;
>wire [7:0] c;
>assign c = {a, b};
>// The MSB of `c` becomes equal to the MSB of `a`, 
>// while the LSB of `c` becomes equal to the LSB of `b`.
>```

- Example, concatinating parts of multiple busses:
>```verilog
>wire [3:0] x, y, z;
>assign z = {x[1:0], y[3:2]};
>// The MSB of `z` becomes equal to the second bit of `x`,
>// while the LSB of `z` becomes equal to the third bit of `y`.
>```
In the example above, the MSB is the second bit of `x` (`x[1]`) while the LSB is the third bit of `y` (`y[2]`).

- Example, setting multiple bits from a single bus:
>```verilog
>wire i, j;
>wire [1:0] k;
>assign {i, j} = k;
>// The MSB of `k` is being assigned to `i`, 
>// while the LSB of `k` is being assigned to `j`. 
>```

```verilog
module concatination_example(
    input [15:0] sw,
    output [8:0] led
);
    
    wire carryOut;
    wire [7:0] a, b, sum;
    
    assign {a, b} = sw;
    assign {carryOut, sum} = a + b;
    assign led = {carryOut, sum}; 
    
endmodule
```
 
## Replication

There is also replication. This is used to repeat the same bit `n` times. Here is an example:
>```verilog
>wire a;
>wire [3:0] b;
>assign b = {4{a}};
>// This assigns `b` to four repeated bits of `a`. 
>
>// For example, if `a = 1'b1`, then `{4{a}} = 4'b1111`.
>```

```verilog
module replication_example(
    input [2:0] sw,
    output [8:0] led
);
   // Here, sw[0] will turn the least significant three bits of led on/off
   assign led[2:0] = {3{sw[0]}};
   
   // Here, sw[1] will turn the middle three bits of led on/off
   assign led[5:3] = {3{sw[1]}};
   
   // Here, sw[0] will turn the most significant three bits of led on/off
   assign led[8:6] = {3{sw[2]}};
    
endmodule
```
[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
