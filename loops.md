[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Loops
There are multiple kinds of loops in Verilog and System Verilog, but some of them are only used in simulations.

| **Loop**  | **Synthesizable** | **Description** |
|:---------:|:-----------------:|-----------------|
| forever   | NO                | Will run the code, literally, forever and can only be stopped by using the statement `$finish` |
| repeat    | YES               | Repeats statement(s) for given number of times |
| while     | NO                | Repeats statement(s) while a condition is true |
| for       | YES               | Similar to the while and repeat loops and can be used to iterate until a condition is false |
| do while  | NO                | Similar to a while loop, but will run the statement(s) at least once |
| foreach   | YES               | Similar to a for loop, but will iterate for each item in an array and then exit |

## Forever Loop
### Syntax 
>```verilog
>forever
>begin
>   // Statements
>end
>```

### Example
>```verilog
>module test();
>   reg clk;
>   
>   initial
>   begin
>     clk = 1'b0;
>   
>     forever
>     begin
>       // Create a clock that oscillates every 5 time units
>       // (Creates a clock with a period of 10 time units).
>       #5 clk = ~clk; 
>     end
>   end
>   
>   // After 50 time units, stop simulation
>   initial
>   begin
>     #50 $finish;
>   end
>endmodule  

## Repeat Loop
### Syntax
>```verilog
>// n is the number of times to repeat the statements
>repeat(<n>)
>begin
>   // Statements
>end
>```
  
### Example
>```verilog
>module test();
>   reg clk;
>   
>   initial
>   begin
>     clk = 1'b0;
>     
>     // Will only let the clock oscillate 10 times or until $finsh is called
>     repeat(10)
>     begin
>       // Create a clock that oscillates every 5 time units
>       // (Creates a clock with a period of 10 time units).
>       #5 clk = ~clk; 
>     end
>   end
>   
>   // After 50 time units, stop simulation
>   initial
>   begin
>     #50 $finish;
>   end
>endmodule  

## While Loop
### Syntax
>```verilog
>while(<condition>)
>begin
>   // Statements
>end
>```
  
### Example
>```verilog
>module test();
>   reg clk;
>   reg [3:0] counter;
>   
>   initial
>   begin
>     clk = 1'b0;
>     counter = 4'h0;
>     
>     // Will only run the statements until counter = 4'hF, 
>     // or until $finsh is called
>     while(counter < 4'hF)
>     begin
>       // Create a clock that oscillates every 5 time units
>       // (Creates a clock with a period of 10 time units).
>       #5 clk = ~clk; 
>       counter = counter + 1;
>     end
>   end
>   
>   // After 50 time units, stop simulation
>   initial
>   begin
>     #50 $finish;
>   end
>endmodule  

## For Loop
### Syntax
>```verilog
>for(<initial_condition>; <condition>, <step_assignment>)
>begin
>   // Statements
>end
>```
  
### Example
>```verilog
>module mem8x8(
>    input clk, write, reset,
>    input [3:0] addr,
>    input [7:0] data_in,
>    output [7:0] data_out
>);
>
>    reg [7:0] memory [0:7];
>    reg [7:0] read_data;
>    integer i;
>    
>    always@(posedge clk)
>    begin
>        if(reset)
>        begin
>          for(i = 0; i < 8; i = i + 1)
>          begin
>            memory[i] = 8'h00;
>          end
>        end
>        
>        else if(write) // Write to address in memory
>        begin
>            memory[addr] = data_in;
>        end
>        
>        else // Read data from address in memory
>        begin
>            read_data = memory[addr];
>        end
>    end
>        
>    assign data_out = read_data;
>
>endmodule
>```

## Do While Loop
### Syntax
>```verilog
>do
>begin
>   // Statements
>end while(<condition>);
>```

### Example
>```verilog
>module test();
>   reg clk;
>   reg [3:0] counter;
>   
>   initial
>   begin
>     clk = 1'b0;
>     counter = 4'h0;
>     
>     // Since counter isn't greater then zero, this loop will only
>     // run once, get to the condition, then exit the loop. 
>     do
>     begin
>       // Create a clock that oscillates every 5 time units
>       // (Creates a clock with a period of 10 time units).
>       #5 clk = ~clk; 
>       counter = counter + 1;
>     end while(counter > 4'h0);
>   end
>   
>   // After 50 time units, stop simulation
>   initial
>   begin
>     #50 $finish;
>   end
>endmodule  

## For Each Loop
### Syntax
>```verilog
>foreach(<variable>[<iterator>])
>begin
>   // Statements
>end
>```

### Example
>```verilog
>module mem8x8(
>    input clk, write, reset,
>    input [3:0] addr,
>    input [7:0] data_in,
>    output [7:0] data_out
>);
>
>    reg [7:0] memory [0:7];
>    reg [7:0] read_data;
>    integer i;
>    
>    always@(posedge clk)
>    begin
>        if(reset)
>        begin
>          foreach(memory[i])
>          begin
>            memory[i] = 8'h00;
>          end
>        end
>        
>        else if(write) // Write to address in memory
>        begin
>            memory[addr] = data_in;
>        end
>        
>        else // Read data from address in memory
>        begin
>            read_data = memory[addr];
>        end
>    end
>        
>    assign data_out = read_data;
>
>endmodule
>```

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
