<pre> `timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08.10.2024 22:04:10
// Design Name: 
// Module Name: SIPI_1M
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
module spi_master (
    input wire clk,             // System clock
    input wire rst,             // Reset
    input wire [7:0] data_in,   // Data to be sent to the slave
    input wire miso,            // Master In Slave Out
    output reg mosi,            // Master Out Slave In
    output reg sclk,            // Serial Clock
    output reg ss,              // Slave Select (Active Low)
    output reg [7:0] data_out   // Data received from the slave
);
    reg [2:0] count;            // 3-bit counter to count 8 bits (0-7)
    reg [7:0] mem;              // Register to store received data
    initial begin
        sclk = 0;
        ss = 1;                 // Initially, deselect the slave
    end

  // Generate SPI Clock (SCLK) and select slave (SS)
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            sclk <= 0;
            ss <= 1;
            count <= 0;
        end else begin
            if (count == 0) ss <= 0; // Activate SS when transmission starts
            sclk <= ~sclk;            // Toggle SPI clock
        end
    end

  // Transmit data on the positive edge of SCLK
    always @(posedge sclk or posedge rst) begin
        if (rst) begin
            count <= 0;
            mosi <= 0;
        end else if (count < 8) begin
            mosi <= data_in[7 - count]; // Transmit MSB first
            count <= count + 1;
        end else begin
            count <= 0;
            ss <= 1;                    // Deactivate SS when transmission ends
        end
    end

  // Receive data on the negative edge of SCLK
    always @(negedge sclk or posedge rst) begin
        if (rst) begin
            data_out <= 0;
            mem <= 0;
        end else if (count > 0 && count <= 8) begin
            mem[7 - count] <= miso;  // Store received bit from slave
        end else if (count == 8) begin
            data_out <= mem;         // Transfer received data to output
        end
    end
endmodule </pre>
