[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Debugging
This page is really just a bunch of seggestions that you can use when debugging HDL code in general. There are many helps in IDEs (like Vivado) that can also help debug code.

This page contains two main sections, [Preventative](https://github.com/Amulek1416/verilog-help-sheet/blob/main/debugging.md#preventative) and [Catching Bugs](https://github.com/Amulek1416/verilog-help-sheet/blob/main/debugging.md#catching-bugs). The preventative section contains ideas of how to reduce posibilities of bugs while the catching bugs section goes over general steps that can be taken to find bugs. 

## Preventative
This section contains great ways to help prevent errors and/or make it easier to read the code and find errors.

As you can see, most of the help has to do with formatting and commenting. A mindset that can when coding in any language is *Can someone look at this code and understand what is going on quickly and efficiently?"*

### Use Named Port Convention
See [Module Instantiation](https://github.com/Amulek1416/verilog-help-sheet/blob/main/module_instantiation.md) under *Named Port Convention*
Not having to worry about the order and really help. It will also help show exactly what is connected to what.

### Keep Good Formatting
Keeping your code nice and neat helps to make it really easy to read the code. 

The following is an example of neat code. The module doesn't really do anything, but shows a good example for formatting.
```verilog
module a_good_looking_module(
   input inputA,
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
   
   another_module_instantiation U0 #(
       .BIT_SIZE(1)
   )(
       .inputA(input_A),
       .outputB(output_B)
   );
   
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

The following is the same code, but without having neat formatting:
```verilog
module a_good_looking_module(input inputA, input [3:0] inputB, input [15:0] inputC, output reg outputA, output [3:0] outputB, output [15:0] outputC);
reg [15:0] memory [0:7];
integer i;
  
initial
begin
for(i = 0; i < 8; i = i + 1)
begin
memory[i] = 16'h0000; 
end
end
   
another_module_instantiation U0 #(.BIT_SIZE(1))(.inputA(input_A),.outputB(output_B));

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
As you can see, keeping your code nice and neat will help you to understand what the code is doing when you come back to it later. It will also help others understand what your code is doing much quicker too.

## Catching Bugs
More information will be coming soon. See the [Preventative](https://github.com/Amulek1416/verilog-help-sheet/blob/main/debugging.md#preventative) section and follow its guidlines. Following these can also help find simple bugs.
[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
