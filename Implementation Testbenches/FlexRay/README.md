<pre>
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 14.03.2025 18:17:02
// Design Name: 
// Module Name: Tb_ray
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
`timescale 1ns/1ps

module tb_flexray_frame();

    reg clk;
    reg reset;
    reg tx_enable;
    reg [15:0] frame_id;
    reg [7:0] cycle;
    reg [7:0] length;
    reg [127:0] payload;
    wire [7:0] tx_data;
    wire tx_valid;

    flexray_frame uut (
        .clk(clk),
        .reset(reset),
        .tx_enable(tx_enable),
        .frame_id(frame_id),
        .cycle(cycle),
        .length(length),
        .payload(payload),
        .tx_data(tx_data),
        .tx_valid(tx_valid)
    );

    always #5 clk = ~clk;  // 10 ns clock period

    initial begin
        clk = 0;
        reset = 1;
        tx_enable = 0;
        frame_id = 16'hA1B2;
        cycle = 8'h05;
        length = 8;
        payload = 128'h112233445566778899AABBCCDDEEFF00;
        
        #20 reset = 0;
        #10 tx_enable = 1;
        
        #200 tx_enable = 0; // Stop transmission
        #50 $stop;
    end

    always @(posedge clk) begin
        $display("Time: %0t | State: %0d | Byte Count: %0d | tx_data: %h | tx_valid: %b",
                 $time, uut.state, uut.byte_count, tx_data, tx_valid);
    end

endmodule




</pre>
