[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Procedural Statements
These are different between [Verilog](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#verilog) and [System Verilog](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#system-verilog)

# Verilog
Procedural statements are used for working with `reg` type variables. It can also be used for flip-flop type logic and behavioral type logic. 

There are two procedural statements in Verilog, [always](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#always) and [initial](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#initial).

## always
An always statement will execute when anything in the sensitivity list changes. There are three ways to write an always statement.

### Syntax
General Syntax
>```verilog
> always@(<sensitivity_list>)
> begin
>   <code_to_execute>
> end
>```

The following is the syntax for creating an always statement that will execute whenever anything in the sensitivity list changes.
>```verilog
> always@(<net_0>, <net_1>, ..., <net_n>)
> begin
>   <code_to_execute>
> end
>```

The following is the syntax for creating an always statement that will execute whenever any signals in the module change value.
>```verilog
> always@*
> begin
>   <code_to_execute>
> end
>```

The following always statement will execute when the items in the sensitivity list transition on a specific edge. To make the signal go on the negative edge, use the command `negedge`. To make a signal go on the positive edge, use the command `posedge`.

**WARNING:** If one item in the synsitivity list is on an edge, all the others must be as well! They can be on different edges, but MUST be on an edge.

>```verilog
> always@(<edge_type> <net_0>, ..., <edge_type> <net_n>)
> begin
>   <code_to_execute>
> end
>```

### Example
Here is a simple example of using an always statement for behavioral logic. It will execute when any net in the sensitivity list changes.
```verilog
/**
 * Module creates a 2:1 MUX with 8-bit busses using an always statement.
 */
module MUX_2_1(
  input s,
  input [7:0] x, y,
  output reg [7:0] f
);

  always@(x, y, s)
  begin
    case(s)
      1'b0 : f = x;
      1'b1 : f = y;
      default : f = x;
    endcase
  end

endmodule
```

Here is a simple example of using an always statement that is triggered on edges:
```verilog
/**
 * This module counts up on every positive edge of the clock. 
 * Setting the reset low will set the counter to zero.
 */
module counter(
  input clk, reset,
  output reg [7:0] count
);
  
  always@(posedge clk, negedge reset)
  begin
    if(~reset) count <= 8'h00;
    else count <= count + 1'b1;
  end

endmodule
```

## initial
Similar to an always statement, except that it isn't synthesizable and is primarily used for setting default values in testbenches as well as used in creating the testbench code. It also doesn't have a sensitivity list.

### Syntax
```verilog
 initial
 begin
   <initialization_code>
 end
```

### Example
```verilog
/**
 * This module creates 4 8-bit memory locations.
 * It uses an initial statement that sets all
 * memory locations to zero when running a 
 * simulation.
 */
module MEM_4x8(
  input clk, write_en, reset,
  input [1:0] addr
  input [7:0] data_in,
  output reg [7:0] data_out
);

  reg [7:0] memory [0:3];
  
  // Initialize everything to zero
  initial
  begin
    memory[0] = 8'h00;
    memory[1] = 8'h00;
    memory[2] = 8'h00;
    memory[3] = 8'h00;
  end
  
  always@(posedge clk)
  begin
    if(reset) 
    begin
      memory[0] <= 8'h00;
      memory[1] <= 8'h00;
      memory[2] <= 8'h00;
      memory[3] <= 8'h00;
    end
    else if(write_en)
    begin
      memory[addr] <= data_in;
    end
    else
    begin
      data_out <= memory[addr];
    end
  end

endmodule
```

# System Verilog
System Verilog is a little more complex when it comes to always statements. Instead of using a single generic always statement, it uses distinct always statements for [flip-flops](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#flip-flops), [combinational logic](), and [latches](). 

**NOTE:** System Verilog uses [initial statements](https://github.com/Amulek1416/verilog-help-sheet/blob/main/procedural_blocks_verilog.md#initial) in the exact same way as Verilog

## Flip-Flops
### Syntax
>```systemverilog
>always_ff@(<condition>)
>begin
>   // Code
>end
>```
### Example

## Combinational Logic
*Coming Soon*
  
## Latches
*Coming Soon*

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
