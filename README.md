# Exp-4-Mux-Demux
# Design and Verification of 2:1 Multiplexer and 1:2 Demultiplexer<br><br><br>



## Aim
To design and verify a 2:1 Multiplexer and a 1:2 Demultiplexer using Verilog HDL.<br><br><br>



## Theory / Synopsis
A 2:1 Multiplexer (MUX) selects one of two data inputs, A or B, and routes it to a single output Y based on the value of the select line S. A 1:2 Demultiplexer (DEMUX) performs the reverse operation: it routes a single data input D to one of two outputs, Y0 or Y1, based on the select line S.<br>

Boolean Functions<br>
### 2:1 Multiplexer:<br>
•	Y = S'.A + S.B<br>
### 1:2 Demultiplexer:<br>
•	Y0 = S'.D<br>
•	Y1 = S.D<br><br><br>



## Files to be Created<br>

<img width="575" height="65" alt="image" src="https://github.com/user-attachments/assets/04c2d90c-0b6b-4b35-b603-d0e10e1e4f00" /><br><br><br>



## Design / RTL Program
```
module mux2to1 (
    input  wire A,      // input line 0
    input  wire B,      // input line 1
    input  wire S,      // select line
    output wire Y       // output
);
    assign Y = (~S & A) | (S & B);
endmodule
 
module demux1to2 (
    input  wire D,      // data input
    input  wire S,      // select line
    output wire Y0,     // output line 0
    output wire Y1      // output line 1
);
    assign Y0 = (~S) & D;
    assign Y1 = S & D;
endmodule

```
<br>


## **Testbench Program
```
module tb4;
    reg  A, B, S, D;
    wire Y;
    wire Y0, Y1;
 
    // Instantiate the 2:1 Multiplexer
    mux2to1 mux_uut (
        .A(A),
        .B(B),
        .S(S),
        .Y(Y)
    );
 
    // Instantiate the 1:2 Demultiplexer
    demux1to2 demux_uut (
        .D(D),
        .S(S),
        .Y0(Y0),
        .Y1(Y1)
    );
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("mux_demux.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb4);             // dump all signals in this testbench hierarchy
 
        // ---- Apply all input combinations ----
        $monitor("Time=%0t S=%b A=%b B=%b Y=%b | D=%b Y0=%b Y1=%b",
                  $time, S, A, B, Y, D, Y0, Y1);
 
        // MUX select = 0 -> Y should follow A
        S = 0; A = 0; B = 0; D = 0; #10;
        S = 0; A = 0; B = 1; D = 1; #10;
        S = 0; A = 1; B = 0; D = 1; #10;
        S = 0; A = 1; B = 1; D = 0; #10;
 
        // MUX select = 1 -> Y should follow B
        S = 1; A = 0; B = 0; D = 0; #10;
        S = 1; A = 0; B = 1; D = 1; #10;
        S = 1; A = 1; B = 0; D = 1; #10;
        S = 1; A = 1; B = 1; D = 0; #10;
 
        #10 $finish;
    end
endmodule

```
<br><br><br>



## Truth Table<br>

### 2:1 Multiplexer

<img width="573" height="154" alt="image" src="https://github.com/user-attachments/assets/96dd3fa2-7747-402d-b88f-17c11e28fcbb" /><br>

### 1:2 Demultiplexer

<img width="583" height="90" alt="image" src="https://github.com/user-attachments/assets/4bc21b52-1db5-4302-afab-ebb0b9d5a74b" /><br><br><br>



## Simulation Procedure<br>

STEP 1 – Open Terminal<br>
Open a terminal in the experiment folder.<br>

STEP 2 – Load Synopsys Environment<br>
source /synopsys/start.sh<br>

STEP 3 – Compile Using VCS<br>
vcs mux_demux.v mux_demux_tb.v -full64<br>
If compilation is successful, VCS generates the simulation executable:<br>
simv<br>

STEP 4 – Run Simulation<br>
./simv<br>
The terminal displays the input combinations and corresponding outputs Y, Y0, and Y1.<br>
A VCD waveform file is also generated:<br>
mux_demux.vcd<br>

STEP 5 – Open DVE<br>

dve -full64<br>
Other option<br>
dve -full64 &<br>
A DVE environment will open.<br>
DVE Waveform Verification<br>
In DVE:<br>
•	Open the testbench hierarchy.<br>
•	Locate the signals:<br>
o	A<br>
o	B<br>
o	S<br>
o	Y<br>
o	D<br>
o	Y0<br>
o	Y1<br>
•	Add the signals to the waveform window.<br>
•	Run/inspect the waveform.<br>
•	Verify that Y = A whenever S = 0, and Y = B whenever S = 1.<br>
•	Verify that Y0 = D and Y1 = 0 whenever S = 0, and Y1 = D and Y0 = 0 whenever S = 1.<br>
•	Confirm that Y0 and Y1 are never both high at the same time, since only one output is active per select value.<br>
The waveform should agree with the truth table.<br><br><br>



## Expected Result<br>

The 2:1 Multiplexer and 1:2 Demultiplexer were realized using Verilog HDL:<br>

•	Y = S'.A + S.B<br>
•	Y0 = S'.D, Y1 = S.D<br>

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.<br><br><br>



## Output<br>
<img width="1842" height="1016" alt="DE 4d" src="https://github.com/user-attachments/assets/b9c3f80b-2c04-418e-aacf-fdabffc993aa" /><br><br><br>



## Viva-Voce Questions

•	What is a Multiplexer, and why is it called a 'data selector'?

•	What is a Demultiplexer, and how does it differ functionally from a Multiplexer?

•	How many select lines are required for an 8:1 MUX and a 1:8 DEMUX?

•	Write the Boolean expression for a 4:1 Multiplexer.

•	What is the purpose of a Verilog testbench?

•	Why is a VCD file generated, and what does $dumpvars(0, tb) do?

•	What is the purpose of ./simv?

•	What is the difference between simulation and synthesis?

•	How would you extend the 2:1 MUX design to a 4:1 MUX using two 2:1 MUXes?<br><br><br>



## Result

Thus, the 2:1 Multiplexer and 1:2 Demultiplexer were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.



