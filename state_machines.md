[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# State Machines

State machines are a core part of digital design. The following is an example of a moore state machine that is detecting a sequence of `110` from incoming serial data. 

```verilog

module find_110(
  input d,
  output found_flg
);

  localparam INIT = 2'b00;
  localparam GOT_1 = 2'b00;
  localparam GOT_11 = 2'b00;
  localparam GOT_110 = 2'b00;
  

endmodule
```

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
