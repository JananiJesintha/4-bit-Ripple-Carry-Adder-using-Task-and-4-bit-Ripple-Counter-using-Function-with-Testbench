 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

Verilog Code
module RippleCarryAdder_4bit (
    input [3:0] a,        
    input [3:0] b,        
    input cin,           
    output [3:0] sum,     
    output cout          
);
    reg [3:0] sum_reg;
    reg carry_out;
    assign sum = sum_reg;
    assign cout = carry_out;
    task full_adder;
        input a_bit, b_bit, carry_in;
        output sum_bit, carry_out_bit;
        begin
            sum_bit = a_bit ^ b_bit ^ carry_in;
            carry_out_bit = (a_bit & b_bit) | (carry_in & (a_bit ^ b_bit));
        end
    endtask
    reg c1, c2, c3;
    always @(*) begin
        full_adder(a[0], b[0], cin, sum_reg[0], c1);
        full_adder(a[1], b[1], c1, sum_reg[1], c2);
        full_adder(a[2], b[2], c2, sum_reg[2], c3);
        full_adder(a[3], b[3], c3, sum_reg[3], carry_out);
    end
endmodule
![Screenshot 2024-11-13 124136](https://github.com/user-attachments/assets/11e7d93d-6f49-4636-b97f-d09b50bdf618)

 Test bench for Ripple carry adder
module RippleCarryAdder_4bit_tb;
    reg [3:0] a, b;
    reg cin;
    wire [3:0] sum;
    wire cout;
    RippleCarryAdder_4bit uut (
        .a(a),
        .b(b),
        .cin(cin),
        .sum(sum),
        .cout(cout)
    );
    initial begin
        a = 4'b0001; b = 4'b0010; cin = 1'b0;
        #10;
        a = 4'b0101; b = 4'b0011; cin = 1'b1;
        #10;
        a = 4'b1111; b = 4'b1111; cin = 1'b1;
        #10;
        a = 4'b1010; b = 4'b0101; cin = 1'b0;
        #10;
        $stop;
    end
endmodule
![Screenshot 2024-11-13 124136](https://github.com/user-attachments/assets/c76dd739-4975-49e2-bd5d-58e2853ea079)

Verilog Code ripple counter
module RippleCounter_4bit (
    input clk,          
    input rst,           
    output [3:0] count   
);
    reg [3:0] count_reg; 
    assign count = count_reg;
    function [3:0] increment;
        input [3:0] value;
        begin
            increment = value + 1;
        end
    endfunction
    always @(posedge clk or posedge rst) begin
        if (rst)
            count_reg <= 4'b0000;           
        else
            count_reg <= increment(count_reg); 
    end
endmodule
![Screenshot 2024-11-13 113732](https://github.com/user-attachments/assets/36c5ce9f-bbe5-4631-8329-4c11119f9f56)

TestBench
module RippleCounter_4bit_tb;
    reg clk;
    reg rst;
    wire [3:0] count;
    RippleCounter_4bit uut (
        .clk(clk),
        .rst(rst),
        .count(count)
    );
    initial begin
        clk = 0;
        forever #5 clk = ~clk; 
    end
    initial begin
        rst = 1;
        #10;
        rst = 0;
        #100;
        rst = 1;
        #10;
        rst = 0;
        #100;
        $stop; 
    end
endmodule
![Screenshot 2024-11-13 113732](https://github.com/user-attachments/assets/503cba9a-9042-4380-b6f1-dc871618fc90)

Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

