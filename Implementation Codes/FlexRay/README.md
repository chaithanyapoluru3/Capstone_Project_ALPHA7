<pre>
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 14.03.2025 18:16:34
// Design Name: 
// Module Name: Flex_ray
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
module flexray_frame (
    input wire clk,            // System Clock
    input wire reset,          // Reset signal
    input wire tx_enable,      // Enable transmission
    input wire [15:0] frame_id,// Frame ID
    input wire [7:0] cycle,    // Cycle Count
    input wire [7:0] length,   // Payload Length
    input wire [127:0] payload,// Data Payload
    output reg [7:0] tx_data,  // Transmitted Data
    output reg tx_valid        // Transmission Valid
);

    reg [7:0] state;
    reg [3:0] byte_count;
    reg [15:0] crc;  // Placeholder CRC calculation (to be implemented later)

    parameter IDLE = 0, SEND_HEADER = 1, SEND_PAYLOAD = 2, SEND_CRC = 3;

    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= IDLE;
            tx_data <= 8'h00;
            tx_valid <= 0;
            byte_count <= 0;
        end else begin
            case (state)
                IDLE: begin
                    tx_data <= 8'h00;  // Ensure tx_data initializes properly
                    tx_valid <= 0;
                    byte_count <= 0;
                    if (tx_enable) begin
                        state <= SEND_HEADER;
                        tx_valid <= 1;
                    end
                end

                SEND_HEADER: begin
                    case (byte_count)
                        0: tx_data <= frame_id[15:8];
                        1: tx_data <= frame_id[7:0];
                        2: tx_data <= cycle;
                        3: tx_data <= length;
                    endcase
                    byte_count <= byte_count + 1;
                    if (byte_count == 3) begin
                        byte_count <= 0; // Reset for payload
                        state <= SEND_PAYLOAD;
                    end
                end

                SEND_PAYLOAD: begin
                    if (byte_count < length) begin
                        tx_data <= (payload >> (120 - byte_count * 8)) & 8'hFF;  // Corrected payload extraction
                        byte_count <= byte_count + 1;
                    end else begin
                        state <= SEND_CRC;
                        byte_count <= 0;
                    end
                end

                SEND_CRC: begin
                    if (byte_count == 0) tx_data <= crc[15:8];
                    else if (byte_count == 1) begin
                        tx_data <= crc[7:0];
                        tx_valid <= 0;
                        state <= IDLE;
                    end
                    byte_count <= byte_count + 1;
                end

            endcase
        end
    end
endmodule



</pre>
