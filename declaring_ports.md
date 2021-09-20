[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Declaring Ports

Ports are what go between the parenthesis of the module (similar to parameters in C programming). These are what connect to the outside of the module. These ports can be three different types: `input`, `output`, and `inout`.

There are three ways to declare ports (shown in the following example modules).

## Declaring Ports Examples
```verilog
/** 
 * This example declares the port names, 
 * but states the port types inside the module
 */
module port_declaration_example_1(
    btnA, 
    btnB,   
    ledA, 
    ledB    
);
    
    input btnA;
    input btnB;
    output ledA;
    output ledB;
    
endmodule
```

```verilog
module port_declaration_example_2(
    input btnA,     // You can declare each individual port as input/output on a separate line
    input btnB,     // As you can see, this helps with commenting each individual port
    output ledA, 
    output ledB    
);

endmodule
```

```verilog
/**
 * This example declares the port names and the port type. 
 * When a port doesn't have a port type, it will use the 
 * port type of the previous port.
 */
module port_declaration_example_3(
    input btnA, btnB,   // All of these are inputs
    output ledA, ledB   // All of these are outputs 
);

endmodule
```
## Declaring Ports Using Busses Example
```verilog
/**
 * This example shows how to have a bus as an input/output/inout
 */
module port_declaration_busses_example(
    input [1:0] buttons,
    output [1:0] leds 
);

endmodule
```

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
