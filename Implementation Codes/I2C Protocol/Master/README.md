`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08.10.2024 16:45:34
// Design Name: 
// Module Name: I2C_V7h
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
module master(
    output reg sda,
    input [7:0] data,
    input [7:0] data_wr,
    input clk,
    input rw,
    output reg scl,
    input [6:0] address,
    input [7:0] register
);

reg [8:0] temp;
reg [7:0] register2;
reg [7:0] data_wr_dup;
reg pstate;
reg scl2x;
reg ack;
integer i;
integer n;

// Initial block to set initial values
initial begin
    i = 0;
    n = 0;
    scl2x = 0;
    ack = 1'b1;
    sda = 1;
    scl = 1;
    #5 sda = 0;  // START BIT condition starts here
end

// Detect start condition
always @(negedge sda) begin
    if (scl == 1)
        n = 1;
end

// Main clock control block
always @(posedge clk) begin
    ack = 0;
    temp = {address, rw, ack};
    register2 = register;
    data_wr_dup = data_wr;

    if (n == 1 && rw == 1) begin
        repeat(50) begin
            #2 scl <= !scl; 
            n = 0;
            #1 scl2x <= !scl2x; 
        end
    end else if (n == 1 && rw == 0) begin
        repeat(64) begin
            #2 scl <= !scl; 
            #1 scl2x <= !scl2x; 
            n = 0;
        end
    end
end

// Control signal for read/write operations
always @(posedge clk) begin
    if (i == 25 && rw == 1)
        repeat(2) #1 scl2x = !scl2x;
    else if (i == 32 && rw == 0)
        repeat(2) #1 scl2x = !scl2x;
end

// I2C bit transmission control
always @(posedge scl2x) begin
    if (i <= 9) begin
        sda = temp[8];
        temp = temp << 1;
    end else if (i == 12 || i == 13) begin
        sda = 1'b0; // Acknowledge bit
    end else if (i >= 14) begin
        sda = register2[7];
        register2 = register2 << 1;
    end
    if (rw == 0 && i >= 23) begin
        sda = data_wr_dup[7];
        data_wr_dup = data_wr_dup << 1;
    end
    i = i + 1;

  // Stop condition
    if (i > 32 && rw == 0)
        sda = 1;
    else if (i > 25 && rw == 1)
        sda = 1;
end

// Instantiate the slave module
slave slv(data, sda, scl);

endmodule
