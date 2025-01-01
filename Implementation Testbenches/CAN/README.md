' `timescale 1ns / 1ps

////////////////////////////////////////////////////////////////////////////////
// Testbench for CAN_Frame module
////////////////////////////////////////////////////////////////////////////////

module CAN_Frame_tb;
    // Inputs
    reg clk;
    reg rst_n;
    reg start_transmission;
    reg [10:0] id;
    reg [7:0] data;

  // Outputs
    wire tx;

  // Instantiate the CAN_Frame module
    CAN_Frame uut (
        .clk(clk),
        .rst_n(rst_n),
        .start_transmission(start_transmission),
        .id(id),
        .data(data),
        .tx(tx)
    );

// Clock generation: 10 ns period
    initial clk = 0;
    always #5 clk = ~clk;  // Clock with 10ns period (50 MHz frequency)

  // Test vector generation
    initial begin
  // Initialize Inputs
        rst_n = 0;
        start_transmission = 0;
        id = 11'd0;
        data = 8'd0;
  // Apply reset for 20 ns
        #20;
        rst_n = 1;
        #20;
   // Set ID and Data for transmission
        id = 11'h7CF;       // Example CAN ID 
        data = 8'hB5;        // Example data byte 

  // Trigger transmission
        start_transmission = 1;
        #10;                 
        start_transmission = 0;

  // Wait for transmission to complete
        #500;               // Adjust the wait time according to FSM timing

   // Finish simulation
        $finish;            // Stop the simulation after the frame is transmitted
    end

  // Monitor signals for debugging
    initial begin
        $monitor("Time=%0t | rst_n=%b | start_transmission=%b | id=0x%h | data=0x%h | tx=%b",
                 $time, rst_n, start_transmission, id, data, tx);
    end

  

endmodule '
