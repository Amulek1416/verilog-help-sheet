# Declaring Ports

Ports are what go between the parenthesis of the module (similar to parameters in C programming). These are what connect to the outside of the module. These ports can be three different types: `input`, `output`, and `inout`.

There are three ways to declare ports (shown in the following example modules).

```verilog
/** 
 * This example declares the port names, 
 * but states the port types inside the module
 */
module port_declaration_example_1(
    btnA, btnB,   
    ledA, ledB    
);
    
    input btnA;
    input btnB;
    output ledA;
    output ledB;
    
endmodule
```

```verilog
/** 
 * This example declares the port names and 
 * the port types for each individual port.
 */
module port_declaration_example_2(
    input btnA, input btnB,   
    output ledA, output ledB    
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
