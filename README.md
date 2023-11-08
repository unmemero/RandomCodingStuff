## Index

[[Code#Pre-requisites|Pre-requisites]]
[[Code#Part 1]|Part 1]]
	[[Code#Equation|Equation]]
	[[Code#Testbench|Testbench]]	
[[Code#Part 2]|Part 2]]
	[[Code#Equation|Equation]]
	[[Code#Testbench|Testbench]]
## Pre-requisites
- Family: Artix 7
- Package: cpg236
- Speed: -1
[[Code#Index|Back to Index]]
## Part 1
### Equation
```verilog

module EQ1(input X,Y,Z,output F,G);
    assign F = (~X & ~Y & Z) | (X & ~Z);
    assign G = (~X | ~Z) & (X | ~Y) & (X | Z);
endmodule
```
[[Code#Index|Back to Index]]
### Testbench
```verilog
module Testbench1;
    reg X,Y,Z;
    wire F,G;
    EQ1 uut(.X(X),.Y(Y),.Z(Z),.F(F),.G(G));
    initial begin
    X=0; Y=0; Z=0;
    #10;
    X=0; Y=0; Z=1;
    #10;
    X=0; Y=1; Z=0;
    #10;
    X=0; Y=1; Z=1;
    #10;
    X=1; Y=0; Z=0;
    #10;
    X=1; Y=0; Z=1;
    #10;
    X=1; Y=1; Z=0;
    #10;
    X=1; Y=1; Z=1;
    #10;
    $stop;
    end
endmodule
```
[[Code#Index|Back to Index]]
## Part 2
### Equation
```verilog
module EQ2(input A,D,W, output B,C);
	assign B = D | W;
	assign C = (A&D) | (A&W);
endmodule
```
[[Code#Index|Back to Index]]
### Testbench
```verilog
module Testbench1;
    reg A,D,W;
    wire B,C;
    EQ2 uut(.A(A),.D(D),.W(W),.B(B),.C(C));
    initial begin
    A=0; D=0; W=0;
    #10;
    A=0; D=0; W=1;
    #10;
    A=0; D=1; W=0;
    #10;
    A=0; D=1; W=1;
    #10;
    A=1; D=0; W=0;
    #10;
    A=1; D=0; W=1;
    #10;
    A=1; D=1; W=0;
    #10;
    A=1; D=1; W=1;
    #10;
    $stop;
    end
endmodule
```
[[Code#Index|Back to Index]]







### LAB 4
```verilog
module Equation(input A,B,C,D, output F,G,Y,Z);
    assign F=C|(~A&D)|(~B&~D);
    assign G=(~C&D)|(A&~B)|(~A&B&~D);
    assign Y=(~A&~B&D)|(C&~D)|(B&~D);
    assign Z=(C&D)|B|(A&D);
endmodule
```
```verilog
module Testbench;
    reg A,B,C,D;
    wire F,G,Y,Z;
    Equation uut(.A(A),.B(B),.C(C),.D(D),.F(F),.G(G),.Y(Y),.Z(Z));
    integer i;
    initial begin
        for(i=0;i<16;i=i+1) begin
            A=i[3];B=i[2];C=i[1];D=i[0];
            #10;
           end
           $stop;
    end
endmodule
```
### LAB 5
#### PART 1
```verilog
module full_adder(input A,B,Cin, output Sum,Cout);
    assign Sum=A^B^Cin;
    assign Cout = (A&B)|(A&Cin)|(B&Cin);
endmodule
```
```verilog
module four_bit_adder(input [3:0] A, B, input Cin, output [3:0] Sum, output Cout);
    wire Cout0, Cout1, Cout2;
    full_adder fa0(.A(A[0]), .B(B[0]), .Cin(Cin), .Sum(Sum[0]), .Cout(Cout0));
    full_adder fa1(.A(A[1]), .B(B[1]), .Cin(Cout0), .Sum(Sum[1]), .Cout(Cout1));
    full_adder fa2(.A(A[2]), .B(B[2]), .Cin(Cout1), .Sum(Sum[2]), .Cout(Cout2));
    full_adder fa3(.A(A[3]), .B(B[3]), .Cin(Cout2), .Sum(Sum[3]), .Cout(Cout));
endmodule
```
```verilog
module Testbench;
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    four_bit_adder  uut(.A(A), .B(B), .Cin(Cin), .Sum(Sum), .Cout(Cout));

    initial begin        
        A <= 4'b0111; B <= 4'b0100; Cin <= 0;
        #10;
        A <= 4'b1010; B <= 4'b0011; Cin <= 0;
        #10;
        A <= 4'b1011; B <= 4'b1001; Cin <= 0;
        #10;
        A <= 4'b1100; B <= 4'b0101; Cin <= 0;
        #10;
        $finish;
    end
endmodule
```
#### PART 2

**NOTE 1**: To run your second testbench, right click on the second testbench in the sources tab, click set as top, and run your simulation.

**NOTE 2**: You require to have in the same project the modules from part 1.
```verilog
module d_flipflop(input D, CLK, output reg Q, Qc);
    always @(posedge CLK)begin
        Q=D;
        Qc=~D;
    end
endmodule
```
```verilog
module four_bit_register(input [3:0] D, input CLK, output [3:0] Q,Qc);
    d_flipflop dff0(D[0],CLK,Q[0],Qc[0]);
    d_flipflop dff1(D[1],CLK,Q[1],Qc[1]);
    d_flipflop dff2(D[2],CLK,Q[2],Qc[2]);
    d_flipflop dff3(D[3],CLK,Q[3],Qc[3]);
endmodule
```
```verilog
module four_bit_adder_register(input [3:0] A,B, input Cin,CLK, output [3:0] Sum, Q,Qc, output Cout);
    four_bit_adder fba(A,B,Cin,Sum,Cout);
    four_bit_register fbr(Sum,CLK,Q,Qc);
endmodule
```
```verilog
module fout_bit_adder_register_testbench;
    reg [3:0] A,B;
    reg Cin, CLK;
    wire [3:0] Sum, Q;
    wire Cout;
    four_bit_adder_register uut(.A(A),.B(B),.Cin(Cin),.CLK(CLK),.Sum(Sum),.Q(Q),.Cout(Cout));
    initial begin
        A=4'b0111;B=4'b0100;Cin=0;CLK=0;
        #20;
        #20;
        CLK=1;
        A=4'b1010;B=4'b0011;
        #20;    
        CLK=0;A=4'b1011;B=4'b1001;
        #20;
        A=4'b1100;B=4'b0101;
        #20;
        $finish;
    end
endmodule
```

### LAB 6
```verilog
module d_flipflop(input d, clk, reset, output reg q);
    always @(posedge clk) begin
        if (reset)
        q=0;
        else
        q=d;
    end
endmodule
```
```verilog
module counter(input clk, reset, output [2:0] presentState,output reg [2:0] nextState);
    d_flipflop dff2(nextState[2],clk,reset,presentState[2]);
    d_flipflop dff1(nextState[1],clk,reset,presentState[1]);
    d_flipflop dff0(nextState[0],clk,reset,presentState[0]);
    always @(presentState) begin
        nextState[2] = (~presentState[2])&(~presentState[1]);
        nextState[1] = (presentState[2])|((~presentState[1])&(~presentState[0]));
        nextState[0] = ((~presentState[2]) & (~presentState[1]))|(presentState[1] & presentState[0]);
    end
endmodule
```
```verilog
module COUNTER_TESTBENCH;
    reg clk, reset;
    wire [2:0] nextState;
    wire [2:0] presentState;
    counter uut(.clk(clk),.reset(reset),.nextState(nextState),.presentState(presentState));
    initial begin
        clk = 0;
        reset = 1;
        #150;
        reset = 0;
        #550;
        $stop;
    end
    always #15 clk = ~clk;
endmodule
```
Second testbench
```verilog
module COUNTER_TESTBENCH;
    reg clk, reset;
    wire [2:0] nextState;
    wire [2:0] presentState;
    counter uut(.clk(clk),.reset(reset),.nextState(nextState),.presentState(presentState));
    initial begin
        clk = 0;
        reset = 1;
        #150;
        reset = 0;
        #250;
	reset = 1;
	#50;
	reset = 0;
	#250;
        $stop;
    end
    always #15 clk = ~clk;
endmodule
```

