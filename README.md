 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

Verilog Code
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


// Test bench for Ripple carry adder

module ripple_carry_adder_4bit_tb;

    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the ripple carry adder
    ripple_carry_adder_4bit uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Test cases
        A = 4'b0001; B = 4'b0010; Cin = 0;
        #10;
        
        A = 4'b0110; B = 4'b0101; Cin = 0;
        #10;
        
        A = 4'b1111; B = 4'b0001; Cin = 0;
        #10;
        
        A = 4'b1010; B = 4'b1101; Cin = 1;
        #10;
        
        A = 4'b1111; B = 4'b1111; Cin = 1;
        #10;

        $stop;
    end

    initial begin
        $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b", $time, A, B, Cin, Sum, Cout);
    end

endmodule


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


Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

