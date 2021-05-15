[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Busses

Busses act similar to arrays in a programming language (but slightly different). 

Busses can be declared for a port, wire, or reg. If a bus is referred to without specifying a specific portion of the bus, then the entire bus will be inferred. 

Portions of the bus can also be referred to, not just a specific bit of the bus. 

The bit order also matters when declaring a bus.

The following is the format for declaring a bus:
>```verilog
><data_type> [<MSB>:<LSB>] <variable_name>
>```

For example, the following bus is 8-bit with bit `0` being the MSB and bit `7` being the LSB:
>```verilog
>wire [0:7] myVar;
>```

And the following bus is also 8-bit, but with bit `7` as the MSB and bit `0` as the LSB:
>```verilog
>wire [7:0] myVar2;
>```


**WARNING**: It is a common practice to have `0` be the LSB and the larger number be the MSB.Unexpected results can happen when combining these two formats.
 
```verilog
module bus_example(
    input [15:0] sw,
    output [8:0] led
);
 
    wire [7:0] a, b;
    
    // assign all 8-bits of `a` to the least 
    // significant 8-bits of `sw`
    assign a = sw[7:0];
    
    // assign all 8-bits of `b` to the most 
    // significant 8-bits of `sw`
    assign b = sw[15:8];
    
    // assign all 9-bits of `led` to the result
    // of adding `a` and `b` together
    assign led = a + b;
 
endmodule
```
[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
