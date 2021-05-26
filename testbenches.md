[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
# Test Benches

Test benches are used to simulate modules. They are especially useful in letting someone work on Verilog code without needing the hardware or having to generate a bitstream everytime there is a small change.

## Implementation
Test benches are implemented using a module with no ports. 

Within the test bench module is where the module to simulate will be instantiated. 

Because test bench modules don't have ports, you need to create your own. For each `input` and `inout` port that connects to the module to simulate, there needs to be a reg declared. Each `output` port that connects to the module to simulate should be declare as a wire.

Here is a small example:
>```verilog
>module and_2bit(
>   input a, b,
>   output c
>);
>    assign c = a & b;
>endmodule
>
>module myTestBench();
>   reg a, b;
>   wire c;
>   
>   and_2bit U0(.a(a), .b(b), .c(c));
>   
>endmodule
>```

## Simulating
Along with creating to ports inside the test bench module, you need to create the logic for the test bench. Since the inputs to the module to simulate are type reg and need to be changed multiple times, they should be set in a procedural statement. Usually this is done in an `initial` statement and ends with a `$finish` statement. 

We also want to have delays so we can see what is going on in a timing diagram. This is done by using a pound (`#`) sign followed by the number of time-units to delay. For example, if we wanted to have a 5 time-unit delay before setting `reg a` high, we would write `#5 a = 1'b1;` in the procedural statement.

Usng the previous example, here is a modified version that shows it with simulation logic:
>```verilog
>module and_2bit(
>   input a, b,
>   output c
>);
>    assign c = a & b;
>endmodule
>
>module myTestBench();
>   reg a, b;
>   wire c;
>   
>   and_2bit U0(.a(a), .b(b), .c(c));
>   
>   initial
>   begin
>       a = 1'b0;
>       b = 1'b0;
>       #5
>       a = 1'b1;
>       b = 1'b0;
>       #5
>       a = 1'b0;
>       b = 1'b1;
>       #5
>       a = 1'b1;
>       b = 1'b1;
>       #5 $finish;
>   end 
>endmodule
>```

This example changes the values of `a` and `b`every 5 time units. Output `c` can be seen in the timing diagram after the simulation is run. It can also be seen by using `$display` to print out the values to the console.

## Example
Supose you need to test the following module:
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

We could then create a test bench like so:
```verilog
// The following sets each time-unit to 1 
// nano-second with a 1 pico-second resolution
`timescale 1ns/1ps 

module registers_testbench(); // Remember, no ports are used in a testbench

  // Inputs (If the signal is an input or inout to the 
  // module we are simulating, it must be of type reg)
  reg clk, reset, write_en, read_en;
  reg [7:0] data_in;
  reg [2:0] address; // This is 3 bits since that will give us the numbers 0-7 (8 possible addresses)
  
  // Outputs (If the signal is an output from the 
  // module we are simulating, it must be of type wire)
  wire [7:0] data_out;
  
  // Integers (Used for loops)
  integer i;
  
  // Instantiate the module we want to simulate. 
  // There should only be one. If we want to 
  // simulate another module, we would want to 
  // make another simulation file with a different 
  // module separate from this one.
  registers U0 #(
    .WIDTH(8),
    .DEPTH(8)
  ) (
    .clk(clk),
    .reset(reset),
    .write_en(write_en),
    .read_en(read_en),
    .addr(address),
    .data_in(data_in),
    .data_out(data_out)
  );

  // initial statements are only executed during 
  // simulations (they aren't synthesizable)
  initial
  begin
    $display("Starting Test Bench\n");
  
    // Set initial values
    clk = 1'b0;
    write_en = 1'b0;
    read_en = 1'b0;
    address = 3'b000;
    
    /**
     * Start with asserting a reset so that 
     * everything within the module being
     * simulated also gets initialized.
     */
    reset = 1'b1; 
    
    /**
     * Since the clock is being pulsed by the always 
     * statement at the bottom of the module, we can 
     * use delays to make sure the values change after
     * a clock pulse. In this case, it would be a delay
     * of 10 time-units
     */
     
    // Here we give some time for the clock to pulse
    // before de-asserting reset.
    #10 reset = 1'b0;
    $display("Finished asserting reset.\n");
    #10 // Delay for another small bit.
    
    // Now we can start setting values and testing.
    
    /**
     * First, we will start by seeing if all addresses 
     * were set to zero during the reset.
     */
    $display("Verifying reset...\n");
    read_en = 1'b1; // State that we are going to read
    for(i = 0; i < 8; i = i + 1)
    begin
      address = i;
      #10 // Delay for a clock cylce to give time to read
      if(data_out != 8'h00)
      begin
        $display("\tERROR: Unable to reset address %d!\n", address);
      end
    end
    read_en = 1'b0; // Disable reading
    $display("Verification Finished!\n");
    
    /**
     * Now we will try to write 8'hBE to address 1.
     */
    $display("Starting single write and read test...\n");
    address = 3'b001;
    data_in = 8'hBE;
    write_en = 1'b1;
    #10 // Delay for a clock cycle
    $display("\tWrote %h at address %d", data_in, address); // Make note to console
    
    // To verify that the write worked, lets read from that address.
    address = 1'b1;
    write_en = 1'b0; // Disable writting
    read_en = 1'b1; // Enable reading
    #10 // Delay to give time for reading
    $display("\tRead %h from address %d", data_out, address); // Display what was read
    
    $display("Finished single read and write test!\n");
    
    // We can also simulate this easier by using loops.
    
    /**
     * First, write to every register address.
     */
    $display("Starting sequential write and read test...\n");
    for(i = 0; i < 8; i = i + 1)
    begin
      /**
       * We will write a different value for each address,
       * starting with 1, so that we can see if we are 
       * writting to every location, one at a time.
       */
      data_in = i + 1; 
      address = i;
      write_en = 1'b1; // Enable writes
      #10 // Delay for a clock cycle
      write_en = 1'b0; // Disable writes
      $display("\tWrote %h to address %d\n", data_in, address);
    end
    
    /**
     * Next we will read from all of the addresses. 
     * If the output isn't what we expect, assert 
     * an error to the console.
     */
    for(i = 0; i < 8; i = i + 1)
    begin
      address = i;
      read_en = 1'b1; // Enable reading
      #10 // Delay for a clock cycle
      read_en = 1'b0; // Disable reading
      
      // Check to make sure the output is what we expect
      if(data_out != i + 1)
      begin
        $display("\tRead %h but expected %h", data_out, i + 1);
      end
    end
    
    $display("Finished sequential read and write test!\n");
    
    
    /**
     * Now, lets double check that we aren't writting to the 
     * registers when write_en is disabled.
     */
    $display("Starting write without write enabled test...\n");
    write_en = 1'b0;
    read_en = 1'b0;
    data_in = 8'hFF;
    for(i = 0; i < 8; i = i + 1)
    begin
      address = i;
      #10 read_en = 1'b1;
      #10 read_en = 1'b0;
      
      if(data_in == data_out)
      begin
        $display("\tERROR: Unexpected write to address %d!", address);
      end
    end
    $display("Finished write without write enabled test!\n");
    
    /**
     * Finally, we can reset everything to make sure reset works 
     * when the entire register is set to non-zeros.
     */
    $display("Starting full reset test...\n");
    write_en = 1'b0;
    read_en = 1'b0;
    reset = 1'b1;
    #10 reset = 1'b0;
    for(i = 0; i < 8; i = i + 1)
    begin
      read_en = 1'b1;
      address = i;
      #10 read_en = 1'b0;
      
      if(data_out != 8'h00)
      begin
        $display("\tERROR: Unable to reset address %d\n", address);
      end
    end
    $display("Finished full reset test!\n");
    
    $display("Finished Test Bench!\n");
    
  end
  
  /**
   * This always statement will pulse a clock every 
   * 5 time-units (the time units are determined by 
   * the timescale above the module). This gives us
   * a clock with a 10 time-unit period.
   */
  always #5 clk = ~clk;

endmodule
```

[[back to Contents]](https://github.com/Amulek1416/verilog-help-sheet/blob/main/README.md)
