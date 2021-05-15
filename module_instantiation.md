# Module Instantiation

Modules can be instantiated in other modules. The following is the basic syntax:
>```verilog
><module_name> <label>(<port_assignments>);
>```

Within this, there are two different styles that can be used, `ordered port convention` and `named port convention`.

## Ordered Port Convention

Ordered port convention is the same as calling a function in most other programming languages. The position of the ports needs to match the position of the ports in the original module. 

For example:

```verilog
/**
 * Module that will be called in the main module
 */
module myModule(
   input x, y, carry_in,
   output sum, carry_out
);

   assign {carry_out, sum} = x + y + carry_in;

endmodule

/**
 * Main module that interfaces with connection to the FPGA chip
 */
module mainModule(
   input [15:0] sw,
   output [8:0] led
);
  wire [7:0] X, Y, SUM;
  wire [6:0] CARRY;
  
  assign X = sw[7:0];
  assign Y = sw[15:8];
  
  /** 
    Since the ports where `myModule` is declared are in the following order;
        `x`, `y`, `carry_in`, `sum`, `carry_out`
        
    the ports need to be in the same order when instantiating the module
  */ 
  
  myModule m0(X[0], Y[0], 1'b0, SUM[0], CARRY[0]);
  myModule m1(X[1], Y[1], CARRY[0], SUM[1], CARRY[1]);
  // ...
  myModule m6(X[6], Y[6], CARRY[4], SUM[6], CARRY[5]);
  myModule m7(X[7], Y[7], CARRY[5], SUM[7], CARRY[6]);
  
  assign led = {CARRY[6], SUM};

endmodule
```

## Named Port Convention

With named port convention, there is no need to worry about the order of the ports. Instead, you can state the name of the port in the module you want to instantiate, then state what that port should be set to.

*FYI*: This may be longer, but can be very beneficial when debugging. This way lets you see the connections of the ports.

Using the same modules as above:

```verilog
/**
 * Module that will be called in the main module
 */
module myModule(
   input x, y, carry_in,
   output sum, carry_out
);

   assign {carry_out, sum} = x + y + carry_in;

endmodule

/**
 * Main module that interfaces with connection to the FPGA chip
 */
module mainModule(
   input [15:0] sw,
   output [8:0] led
);
  wire [7:0] X, Y, SUM;
  wire [6:0] CARRY;
  
  assign X = sw[7:0];
  assign Y = sw[15:8];
  
  /** 
    Since the port names are as follows: 
        `y`, `x`, `sum`, `carry_out`, `carry_in`
        
    the ports need to be reference like so:
        myModule label(.x(<connection_to_x), ..., .sum(<connection_to_sum>));
  */ 
  
  myModule m0(.sum(SUM[0]), .y(Y[0]), .x(X[0]), .carry_in(1'b0), .carry_out(CARRY[0]));
  myModule m0(.y(Y[1]), .sum(SUM[1]), .x(X[1]), .carry_in(CARRY[0]), .carry_out(CARRY[1]));
  // ...
  myModule m6(.y(Y[6]), .x(X[6]), .sum(SUM[6]), .carry_in(CARRY[4]), .carry_out(CARRY[5]));
  
  // Another format that this can be done in:
  myModule m7(
    .y(Y[7]),  
    .x(X[7]), 
    .carry_in(CARRY[5]), 
    .sum(SUM[7]),
    .carry_out(CARRY[6])
  );
  // This format can make it easier to read when there are a large number of ports
  
  assign led = {CARRY[6], SUM};

endmodule
```


