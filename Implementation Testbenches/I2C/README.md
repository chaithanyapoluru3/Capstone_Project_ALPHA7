''' `timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08.10.2024 16:47:59
// Design Name: 
// Module Name: 12C_V7_tb
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////
module tbmast;
    // Inputs
    reg [6:0] address;
    reg [7:0] register;
    reg [7:0] data;
    reg [7:0] data_wr;
    reg clk;
    reg rw;

  // Outputs
    wire sda;
    wire scl;

  // Instantiate the Unit Under Test (UUT)
    master uut (
        .address(address),
        .register(register),
        .clk(clk),
        .rw(rw), 
        .sda(sda),
        .scl(scl),
        .data(data),
        .data_wr(data_wr)
    );
    initial begin
        // Initialize Inputs
        address = 7'b1101001; // Corrected binary declaration
        register = 7'b0100101;
        data = 7'b1011010; // Corrected binary declaration
        clk = 0;
        rw = 0;
        data_wr = 20;

  // Wait 100 ns for global reset to finish
        #100;

  // Add stimulus here
    end

   // Generate clock signal
    always
        #1 clk = ~clk; // Toggle clock every 1 time unit
endmodule
