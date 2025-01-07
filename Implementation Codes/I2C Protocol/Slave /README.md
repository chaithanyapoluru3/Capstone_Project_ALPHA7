<pre> `timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08.10.2024 16:46:43
// Design Name: 
// Module Name: I2C_V7s
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
module slave(
    output reg [7:0] out,
    input sda,
    input scl
);
    integer j = 0;
    reg [6:0] temp;
    reg [7:0] add;
    reg rw;
    reg [7:0] register_address;
    reg bitin;
    reg [7:0] storage[0:38];
    initial begin
        // Initialize storage with a value at index 37
        storage[37] = 16;
    end
    parameter address = 7'b1101001;  // Corrected parameter declaration
    always @(posedge scl) begin
        bitin = sda;
        if (j < 8)
            temp = {temp, bitin}; // Shift in the bit
        if (j == 8) begin
            // Determine read/write based on the ACK bit
            if (bitin == 0)
                rw = 0;  // Write operation
            else
                rw = 1;  // Read operation
        end
        j = j + 1;

  // Check if the address matches for read operations
        if (temp == address && (j > 15 && j < 24) && rw == 1) begin
            add = {add, bitin};
        end
        
  // Check if the address matches for write operations
        if (temp == address && rw == 0 && j > 15 && j != 24 && j < 33) begin
            add = {add, bitin};
        end

  // Store the register address at the appropriate time
        if (j == 24) 
            register_address = add;

  // Store the data in storage if it's a write operation
        if (j == 33 && rw == 0) 
            storage[register_address] = add;

  // Output the value from the storage
        out = storage[add];
    end
endmodule </pre>
