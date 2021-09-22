[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Debugging
This page is really just a bunch of seggestions that you can use when debugging HDL code in general. There are many helps in IDEs (like Vivado) that can also help debug code.

## Prevention
This section contains great ways to help prevent errors and/or make it easier to read the code and find errors.

### Use Named Port Convention
See [Module Instantiation](https://github.com/Amulek1416/verilog-help-sheet/blob/main/module_instantiation.md) under *Named Port Convention*
Not having to worry about the order and really help. It will also help show exactly what is connected to what.

### Keep Good Formatting
Keeping your code nice and neat helps to make it really easy to read the code. 

The following is an example of neat code. The module doesn't really do anything, but shows a good example for formatting.
```verilog
module a_good_looking_module(
   input inputA
   input [3:0] inputB,
   input [15:0] inputC,
   output reg outputA,
   output [3:0] outputB,
   output [15:0] outputC
);

   reg [15:0] memory [0:7];
   integer i;
  
   initial
   begin
      for(i = 0; i < 8; i = i + 1)
      begin
         memory[i] = 16'h0000;
      end
   end
   
   another_module
   
   always@(posedge inputA)
   begin
      if(inputB == 4'b0110)
      begin
         outputA <= 1'b1;
      end
      else
      begin
         outputA <= 1'b0;
      end
   end

endmodule
```




[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
