[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Data Types

## Verilog
### reg & wire
There are two main data types in Verilog: `wire` and `reg`.

`wire`:  Continuously changes based on assignment and is set using an assign statement. All ports are implied to be a wire (unless specifically specified to be a reg).

>Setting a wire: 
>```verilog 
>assign <var_name> = <value>;
>```

`reg`:   Only changes when certain conditions are met. (ex: on the positive edge of a clock). It can only be assigned in a behavioral (always) statement.
>Setting a reg: 
>```verilog
>always@(<condition>)
>   <var_name> = <value>;
>```


```verilog
module wire_example(
    input btnA, btnB,
    output ledA
);
    
    wire isBtnsPressed;
    
    assign isBtnsPressed = btnA & btnB;
    assign ledA = isBtnsPressed;
    
endmodule
```
```verilog
module reg_example(
    input btnA, btnB, clk, // inputs
    output ledA
);
    
    reg turnLedOn;
    
    // This will check if the ledA should be 
    // turned on at every positive edge of
    // the clock
    always@(posedge clk)
        if(btnA && btnB)
            turnLedOn = 1'b1;
        else
            turnLedOn = 1'b0;
    
    assign ledA = turnLedOn;
    
endmodule
```
    
### Other Data Types
    
More information about how to use anything in the following table can be found with a quick google search
    
| **Data Type** | **Description** |
|:---------:|-----------------|
|integer| *Coming Soon* |
|genvar| *Coming Soon* |
|localparam| *Coming Soon* |
    
## System Verilog

Any data types supported in Verilog are also supported in System Verilog. System Verilog, however, supports another data type called `logic`. This data type makes it where you don't need to worry about what needs to be type `reg` or type `wire`.
    
```systemverilog
module logic_as_wire_example(
    input btnA, btnB,
    output ledA
);
    
    logic isBtnsPressed;
    
    assign isBtnsPressed = btnA & btnB;
    assign ledA = isBtnsPressed;
    
endmodule
```
```systemverilog
module logic_as_reg_example(
    input btnA, btnB, clk, // inputs
    output ledA
);
    
    logic turnLedOn;
    
    // This will check if the ledA should be 
    // turned on at every positive edge of
    // the clock
    always_ff@(posedge clk)
        if(btnA && btnB)
            turnLedOn = 1'b1;
        else
            turnLedOn = 1'b0;
    
    assign ledA = turnLedOn;
    
endmodule
```
    
[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
