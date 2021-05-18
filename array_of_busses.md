[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Making An Array of Busses
Verilog will let you make an array of busses. This is very useful when implementing something like a register where you might what *m*x*n*-bit registers. 

## Declaring The Array
The following is the general syntax for an array of size *m* that has *n*-bit busses:
>```verilog
> reg [<n>:0] myArray [0:<m>];
>```

Arrays can also be multi-dimensional. For example, if you want a *j* by *k* array of *n*-bit busses, you would do the following:
>```verilog
> wire [<n>:0] myOtherArray [0:<j>] [0:<k];
>```

Note that these arrays can be declared as `wire` or `reg`.

**WARNING** While you can make the array with `0` being the least significant number, most people use `0` as the most significant bit instead. Be sure watch out for miss-matches with this.

**WARNING**: Using an array of busses as a port only works in System Verilog and **not** Verilog.

## Using The Array
The following can be used to reference a single bus from an array of busses:
>```verilog
> wire [7:0] array [0:4];
> assign array[0] = 8'h00;
>```

The following can be used to reference the first four bits of the first buss in an array of busses:
>```verilog
> wire [7:0] array [0:4];
> assign array[0][3:0] = 4'b1001;
>```

The following is an example of referencing a single buss from a multi-dimentional array of busses:
>```verilog
> wire [7:0] array [0:7][0:1];
> assign array[0][6] = 8'h50;
>```

**WARNING** You can't set an entire array using concatination. This must be done by using a for loop!

## Example
```verilog
module mem8x8(
    input clk, write,
    input [3:0] addr,
    input [7:0] data_in,
    output [7:0] data_out
);

    reg [7:0] memory [0:7];
    reg [7:0] read_data;
    
    always@(posedge clk)
    begin
        if(write) // Write to address in memory
        begin
            memory[addr] = data_in;
        end
        
        else // Read data from address in memory
        begin
            read_data = memory[addr];
        end
    end
        
    assign data_out = read_data;

endmodule
```

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
