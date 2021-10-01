[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Parameters
Parameters are a way to make your code more dynamic in its application. While they can't be modified at runtime, they can be set when instantiating modules.

For example, if I wanted to make a register module that could be set to any width and depth, you could use parameters that will set the width and depth of the registers.

```verilog
module registers #(
    parameter WIDTH=8,
    parameter DEPTH=8
  ) (
      input clk, write_en, read_en, reset,
      input [(DEPTH-1):0] addr,
      input [(WIDTH-1):0] data_in,
      output [(WIDTH-1):0] data_out
);

  reg [(WIDTH-1):0] memory [0:(DEPTH-1)];
  reg [(WIDTH-1):0] read_data;
  
  integer i; // Used in the for loop when reset is asserted
  
  always@(posedge clk)
  begin
    if(reset)
    begin
      for(i = 0; i < DEPTH; i = i + 1)
      begin
        memory[i] = {WIDTH{1'b0}};
      end
    end
    else if(write_en)
    begin
      memory[addr] <= data_in;
    end
    else if(read_en)
    begin
      read_data <= memory[addr];
    end
    else
    end
      read_data <= read_data; // Don't do anything
    begin
  end

  assign data_out = read_data;

endmodule  
```

The module can now be instantiated like normal, keeping the default parameter values, or be instantiated with different parameter values.
```verilog
module top(
    input clk, reset_btn, write_btn, read_btn,
    input [1:0] address,
    input [15:0] save_value,
    output [15:0] read_value
);

    registers U0 #(
        .WIDTH(16),
        .DEPTH(4)
    )(
        .clk(clk),
        .read_en(read_btn),
        .write_en(write_btn),
        .data_in(save_value),
        .addr(address),
        .data_out(read_value),
        .reset(reset_btn)
    );

endmodule
```
The above module will create 4, 16-bit registers by simply modifying the parameters values

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
