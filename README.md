//#** 8-bit-ALU**
//Design and Simulation of an 8-Bit Arithmetic Logic Unit (ALU) in Verilog
//**RTL CODE**
module alu_8bit (
    input [7:0] A,        
    input [7:0] B,        
    input [3:0] ALU_SEL,  
    output reg [7:0] ALU_OUT, 
    output reg ZERO,      
    output reg CARRY      
);
always @(*) begin
    case (ALU_SEL)
        4'b0000: {CARRY, ALU_OUT} = A + B;        
        4'b0001: {CARRY, ALU_OUT} = A - B;        
        4'b0010: ALU_OUT = A & B;                 
        4'b0011: ALU_OUT = A | B;                 
        4'b0100: ALU_OUT = A ^ B;                 
        4'b0101: ALU_OUT = ~A;                    
        4'b0110: ALU_OUT = A << 1;                
        4'b0111: ALU_OUT = A >> 1;                
        4'b1000: {CARRY, ALU_OUT} = A + 1;        
        4'b1001: {CARRY, ALU_OUT} = A - 1;        
        default: ALU_OUT = 8'b00000000;           
    endcase
    if (ALU_OUT == 8'b00000000)
        ZERO = 1;
    else
        ZERO = 0;
end
endmodule

//**testbench**

`timescale 1ns / 1ps

module tb_alu_8bit;
    reg [7:0] A, B;         
    reg [3:0] ALU_SEL;       
    wire [7:0] ALU_OUT;      
    wire ZERO, CARRY;        
    alu_8bit uut (
        .A(A),
        .B(B),
        .ALU_SEL(ALU_SEL),
        .ALU_OUT(ALU_OUT),
        .ZERO(ZERO),
        .CARRY(CARRY)
    );
    initial begin
        $monitor("Time=%0dns | A=%b B=%b ALU_SEL=%b | OUT=%b ZERO=%b CARRY=%b",
                  $time, A, B, ALU_SEL, ALU_OUT, ZERO, CARRY);
        A = 8'b00001111; B = 8'b00000001; ALU_SEL = 4'b0000; #10;
        A = 8'b00001111; B = 8'b00000101; ALU_SEL = 4'b0001; #10;
        A = 8'b11001100; B = 8'b10101010; ALU_SEL = 4'b0010; #10;
        A = 8'b11001100; B = 8'b10101010; ALU_SEL = 4'b0011; #10;
        A = 8'b11001100; B = 8'b10101010; ALU_SEL = 4'b0100; #10;
        A = 8'b11110000; B = 8'b00000000; ALU_SEL = 4'b0101; #10;
        A = 8'b00001111; ALU_SEL = 4'b0110; #10;
        A = 8'b11110000; ALU_SEL = 4'b0111; #10;
        A = 8'b00000001; ALU_SEL = 4'b1000; #10;
        A = 8'b00000001; ALU_SEL = 4'b1001; #10;
        $finish;
    end
endmodule
