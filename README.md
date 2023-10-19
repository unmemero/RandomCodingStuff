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
