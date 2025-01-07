<pre> `timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08.10.2024 22:26:56
// Design Name: 
// Module Name: SPI_1S
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
module spi_slave (
    input wire clk,              // System clock
    input wire rst,              // Reset
    input wire mosi,             // Master Out Slave In
    input wire sclk,             // Serial Clock from Master
    input wire ss,               // Slave Select (Active Low)
    output reg miso,             // Master In Slave Out
    input wire [7:0] data_in,    // Data to be sent to the master
    output reg [7:0] data_out    // Data received from the master
);
    reg [2:0] count;             // 3-bit counter to count 8 bits (0-7)
    reg [7:0] mem;               // Register to store received data
    initial begin
        miso = 0;
    end

  // Receive data from the master on the positive edge of SCLK
    always @(posedge sclk or posedge rst) begin
        if (rst) begin
            count <= 0;
            data_out <= 0;
        end else if (!ss) begin      // Only receive when SS is low (slave selected)
            if (count < 8) begin
                mem[7 - count] <= mosi;  // Receive bit from master
                count <= count + 1;
            end else begin
                data_out <= mem;     // Transfer received data to output
                count <= 0;          // Reset counter for next transmission
            end
        end
    end

  // Transmit data to the master on the negative edge of SCLK
    always @(negedge sclk or posedge rst) begin
        if (rst) begin
            miso <= 0;
        end else if (!ss && count < 8) begin
            miso <= data_in[7 - count];  // Send bit to master
        end
    end
endmodule </pre>
