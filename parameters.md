[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Parameters
Parameters are a way to make your code more dynamic in its application. While they can't be modified at runtime, they can be set when instantiating modules.

For example, if I wanted to make a register module that could be set to any width and depth, I could use parameters that will set the width and depth of the registers.

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
  
  always@(posedge clk)
  begin
    if(reset)
    begin
      // Reset all of memory
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

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
