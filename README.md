# 4-bit-Carry-Look-Ahead-Adder
This repository presents the design of 4-bit carry look ahead adder using mixed signal mode. 
## Table of contents
#### 1.[ABSTRACT](https://github.com/pavang19/Half_Subtractor-#1-abstract)
#### 2.[TOOLS USED](https://github.com/pavang19/Half_Subtractor-#2esim-eda-tool-1)
#### 3.[Sky130 PDK](https://github.com/pavang19/Half_Subtractor-#3sky-130-pdk)
#### 4.[CIRCUIT DESIGN](https://github.com/pavang19/Half_Subtractor-#4circuit-design-1)
#### 5.[IMPLEMENTATION](https://github.com/pavang19/Half_Subtractor-#5implementation-1)
#### 6.[RESULTS](https://github.com/pavang19/Half_Subtractor-#6results-1)
#### 7.[REFERENCES](https://github.com/pavang19/Half_Subtractor-#7references-1)
#### 8.[ACKNOWLEDGEMENT](https://github.com/pavang19/Half_Subtractor-#8acknowledgement-1)
#### 7.[AUTHOR](https://github.com/pavang19/Half_Subtractor-#9author)

### 1. ABSTRACT
This project proposes  the design of a 4-bit carry look ahead adder.A carry-lookahead adder improves speed by reducing the amount of time required to determine carry bits. It can be contrasted with the simpler, but usually slower, ripple-carry adder (RCA), for which the carry bit is calculated alongside the sum bit, and each stage must wait until the previous carry bit has been calculated to begin calculating its own sum bit and carry bit. The carry-lookahead adder calculates one or more carry bits before the sum, which reduces the wait time to calculate the result of the larger-value bits of the adder.In this project the digital part of the circuit is designed using Hardware Description Language (HDL) Verilog on makerchip.A model is created using verilator and finally the  circuit schematic is designed on eSim, an open source EDA tool and simulated on an open source spice simulater Ngspice.

### 2.TOOLS USED

#### 2.1eSIM EDA TOOL
eSim is a free/libre and open source EDA tool for circuit design, simulation, analysis and PCB design. It is an integrated tool built using free/libre and open source software such as KiCad, Ngspice and GHDL.eSim offers similar capabilities and ease of use as any equivalent proprietary software for schematic creation, simulation and PCB design, without having to pay a huge amount of money to procure licenses. <br /> 
Main features of eSim- <br />
- Draw circuits using KiCad, create a netlist and simulate using Ngspice. <br />
- Design PCB layouts and generate Gerber files using KiCad. <br />
- Add/Edit device models(Spice Models) and subcircuits using the Model Builder and Subcircuit Builder tools. <br />
- Perform Mixed-Signal Simulation. <br />
- Support for Ubuntu OS and Windows OS <br />
- It allows the creation and modification of components and symbol libraries.<br />
- Apart from Ngspice plot eSim provides an interactive plotting frontend where user can select nodes or branch and plot voltage or current respectively.<br />
For more information: https://esim.fossee.in/home

#### 2.2 Makerchip
It is an Open-source SystemVerilog/TL-Verilog development platform developed by [Redwood EDA LLC ](http://redwoodeda.com/) Makerchip provides free and instant access to the latest tools from your browser and from your desktop. This includes open-source tools and proprietary ones.You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, waveforms, and novel visualization capabilities are tightly integrated for a seamless design experience.

For more information:-https://www.makerchip.com/

#### 2.3 Verilator
Verilator is a free and open-source software tool which converts Verilog (a hardware description language) to a cycle-accurate behavioral model in C++ or SystemC. It is restricted to modeling the synthesizable subset of Verilog and the generated models are cycle-accurate, 2-state, with synthesis (zero delay) semantics. As a consequence, the models typically offer higher performance than the more widely used event-driven simulators, which can process the entire Verilog language and model behavior within the clock cycle. Verilator is now used within academic research, open source projects and for commercial semiconductor development.

For more information:-https://www.veripool.org/verilator/


### 3.CIRCUIT DESIGN
The 4 bit carry lookahead adder take two inputs x and y each of four bit. They work by creating two signals pi and gi known to be Carry Propagator and Carry Generator. The carry propagator is propagated to the next level whereas the carry generator is used to generate the output carry ,regardless of input carry.The truth table is shown below

![image](https://user-images.githubusercontent.com/55171083/157090857-096f79ad-be76-498c-8375-a91bbd82049b.png)

The region marked in green colour is refereed to as Carry kill , since the carry out is zero regardless of carry in when the inputs x and y are zero.

Carry kill : ki = xi'* yi' 

The regieon marked in red colour is carry propagate. The carry out is same as the carry in when either of the input is logic high hence it is referred as carry propagate.

Carry propagate : pi = xi ^ yi  

The region marked in blue is the carry generate as the carry out is generated when both the inputs are high

Carry generate : gi = xi * yi

The circuit diagram is shown below

![image](https://user-images.githubusercontent.com/55171083/157090075-312f4004-c07e-4c03-b275-ab9acdaab23d.png)

Now the Carry for all the bits is generated by the carry look ahead generator as shown in the below circuit diagram.

Adder Equations :-  

c(i+1) = gi + pi.ci

si = pi ^ ci

Equations of carry look ahead generator

c1= g0 + p0.c0

c2= g1 + p1.(g0 + p0.c0)= g1 + p1.g0 + p1.p0.c0

c3= g2 + p2(g1 + p1.g0 + p1.p0.c0) = g2 + p2.g1 + p2.p1.g0 +p2.p1.p0

c4= g3 + p3.g2 + p3.p2.g1 + p3.p2.p1.g0 +p3.p2.p1.p0.c0


REFERENCE CIRCUIT DIAGRAM

![ref circuit (1)](https://user-images.githubusercontent.com/55171083/157295002-a5656a95-6d68-41b2-a541-fe6aa3c2a3a9.jpeg)



REFERENCE WAVEFORM

![ref waveform](https://user-images.githubusercontent.com/55171083/157289826-96c2c968-a97e-4c58-9356-b518f932e959.jpg)


### 4.IMPLEMENTATION 

The digital block is designed using a Hardware descriptive language (HDL) verilog using makerchip.

#### VERILOG CODE
```
\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/    /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/      /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/    

//Your Verilog/System Verilog Code Starts Here:

module Cla_adder(sum, c_out, a, b, c_in
    );
// Inputs and outputs
output [3:0] sum;
output c_out;
input [3:0] a,b;
input c_in;
// Internal wires
wire p0,g0, p1,g1, p2,g2, p3,g3;
wire c4, c3, c2, c1;
// compute the p for each stage
assign p0 = a[0] ^ b[0],
p1 = a[1] ^ b[1],
p2 = a[2] ^ b[2],
p3 = a[3] ^ b[3];
// compute the g for each stage
assign g0 = a[0] & b[0],
g1 = a[1] & b[1],
g2 = a[2] & b[2],
g3 = a[3] & b[3];
// compute the carry for each stage
// c_in is equivalent c0 in the arithmetic equation for CLA computation
assign c1 = g0 | (p0 & c_in),
c2 = g1 | (p1 & g0) | (p1 & p0 & c_in),
c3 = g2 | (p2 & g1) | (p2 & p1 & g0) | (p2 & p1 & p0 & c_in),
c4 = g3 | (p3 & g2) | (p3 & p2 & g1) | (p3 & p2 & p1 & g0) |
(p3 & p2 & p1 & p0 & c_in);
// Compute Sum
assign sum[0] = p0 ^ c_in,
sum[1] = p1 ^ c1,
sum[2] = p2 ^ c2,
sum[3] = p3 ^ c3;

// Assign carry output
assign c_out = c4;



endmodule


//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  [3:0] sum;//output
		logic  c_out;//output
		logic  [3:0] a;//input
		logic  [3:0] b;//input
		logic  c_in;//input
//The $random() can be replaced if user wants to assign values
		assign a = 4'd12;
		assign b = 4'd02;
		assign c_in =1'b0;
		Cla_adder Cla_adder(.sum(sum), .c_out(c_out), .a(a), .b(b), .c_in(c_in));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule
```


##### MAKERCHIP SIMULATION RESULTS

![makerchip](https://user-images.githubusercontent.com/55171083/157291865-01b836be-66d0-4c4d-af11-4595322357f5.JPG)

#### NgVeri MODEL

After verifying the verilog code and the design functionality now the model is creted by running the verilog to ngspice converter.
Steps to generate the model is given below:-

- Open eSim
- Run NgVeri-Makerchip
- Add top level verilog file in Makerchip Tab
- click on Edit in makerchip to simulate the verilog code
- Click on NgVeri tab
- Add dependency files(If any)
- Click on Run Verilog to NgSpice Converter
- Debug if any errors
- Model will be  created successfully

#### SCHEMATIC 

Now the model is added to the library , so we can use our model in the schematic.In this schematic first the input pulse is given to Analog to digital converter ADC and then the output of ADC is fed as input to the 4 bit CLA adder and the ouput is oubtained after passing through the DAC block.

![schematic](https://user-images.githubusercontent.com/55171083/157298371-4ac21452-6997-4176-8291-3c4b225d8a64.JPG)

After the schematic is done the netlist is generated and the kicad to ngspice converter is used to generate the .cir.out file

#### NETLIST

```
* /home/pavan.gutti2/desktop/pavang_cla_adder/pavang_cla_adder.cir

* u18  net-_u12-pad10_ net-_u12-pad11_ net-_u12-pad12_ net-_u12-pad13_ net-_u12-pad14_ o0 o1 o2 o3 o4 dac_bridge_5
* u12  net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ net-_u10-pad8_ net-_u11-pad6_ net-_u11-pad7_ net-_u11-pad8_ net-_u11-pad9_ net-_u11-pad10_ net-_u12-pad10_ net-_u12-pad11_ net-_u12-pad12_ net-_u12-pad13_ net-_u12-pad14_ pavan_cla_adder
* u10  i0 i1 i2 i3 net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ net-_u10-pad8_ adc_bridge_4
* u11  i4 i5 i6 i7 i8 net-_u11-pad6_ net-_u11-pad7_ net-_u11-pad8_ net-_u11-pad9_ net-_u11-pad10_ adc_bridge_5
v0  i0 gnd pulse(0 5 0.1m 0.1m 0.1m 1 2)
v1  i1 gnd pulse(0 5 0.1m 0.1m 0.1m 1 3)
v2  i2 gnd pulse(0 5 0.1m 0.1m 0.1m 1 4)
v3  i3 gnd pulse(0 5 0.1m 0.1m 0.1m 1 5)
v7  i7 gnd pulse(0 5 0.1m 0.1m 0.1m 1 6)
v6  i6 gnd pulse(0 5 0.1m 0.1m 0.1m 1 7)
v5  i5 gnd pulse(0 5 0.1m 0.1m 0.1m 1 8)
v4  i4 gnd pulse(0 5 0.1m 0.1m 0.1m 1 9)
v8  i8 gnd pulse(0 5 0.1m 0.1m 0.1m 1 10)
* u5  i4 plot_v1
* u6  i5 plot_v1
* u7  i6 plot_v1
* u9  i8 plot_v1
* u1  i0 plot_v1
* u2  i1 plot_v1
* u3  i2 plot_v1
* u4  i3 plot_v1
* u8  i7 plot_v1
r1  o0 gnd 1k
r3  o2 gnd 1k
r4  o3 gnd 1k
r5  o4 gnd 1k
r2  o1 gnd 1k
* u13  o0 plot_v1
* u14  o1 plot_v1
* u15  o2 plot_v1
* u17  o4 plot_v1
* u16  o3 plot_v1
a1 [net-_u12-pad10_ net-_u12-pad11_ net-_u12-pad12_ net-_u12-pad13_ net-_u12-pad14_ ] [o0 o1 o2 o3 o4 ] u18
a2 [net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ net-_u10-pad8_ ] [net-_u11-pad6_ net-_u11-pad7_ net-_u11-pad8_ net-_u11-pad9_ ] [net-_u11-pad10_ ] [net-_u12-pad10_ net-_u12-pad11_ net-_u12-pad12_ net-_u12-pad13_ ] [net-_u12-pad14_ ] u12
a3 [i0 i1 i2 i3 ] [net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ net-_u10-pad8_ ] u10
a4 [i4 i5 i6 i7 i8 ] [net-_u11-pad6_ net-_u11-pad7_ net-_u11-pad8_ net-_u11-pad9_ net-_u11-pad10_ ] u11
* Schematic Name:                             dac_bridge_5, NgSpice Name: dac_bridge
.model u18 dac_bridge(out_low=0.0 out_high=5.0 out_undef=0.5 input_load=1.0e-12 t_rise=1.0e-9 t_fall=1.0e-9 ) 
* Schematic Name:                             pavan_cla_adder, NgSpice Name: pavan_cla_adder
.model u12 pavan_cla_adder(rise_delay=1.0e-9 fall_delay=1.0e-9 input_load=1.0e-12 instance_id=1 ) 
* Schematic Name:                             adc_bridge_4, NgSpice Name: adc_bridge
.model u10 adc_bridge(in_low=1.0 in_high=2.0 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
* Schematic Name:                             adc_bridge_5, NgSpice Name: adc_bridge
.model u11 adc_bridge(in_low=1.0 in_high=2.0 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.tran 0.1e-00 20e-00 0e-00

* Control Statements 
.control
run
print allv > plot_data_v.txt
print alli > plot_data_i.txt
 
plot v(i0) v(i1)+6 v(i2) +12 v(i3)+18 v(i4)+24 v(i5)+30 v(i6)+36 v(i7)+42 v(i8)+48 v(o0)+54 v(o1)+60 v(o2)+66 v(o4)+72 v(o3)+78
.endc

```

### 5.RESULTS 

 The circuit is simulated using Ngspice simulator .
 
 ![spice_simulation](https://user-images.githubusercontent.com/55171083/157300437-de280794-3139-4f36-965e-38456256e7ee.JPG)

<br>
<br>

 
NGSPICE WAVEFORM



![Ngspice_waveform](https://user-images.githubusercontent.com/55171083/157300812-8d85f7e0-e8aa-4b0e-a4b2-64f11aa88671.JPG)



The above waveform matches with the reference waveform and the truth table ,hence the design is implemented successfully and verified.



### 6.REFERENCES

[1] Verilog Digital  Design Synthesis by samir palnitkar   <br />
[2] Digital system design using Verilog by Peter Ashenden <br />


### 7.ACKNOWLEDGEMENT
* FOSSEE, IIT Bombay
* Steve Hoover, Founder, Redwood EDA
* [Kunal Ghosh](https://github.com/kunalg123), Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd 
* [Sumanto Kar](https://github.com/Eyantra698Sumanto),IIT BOMBAY


### 8.AUTHOR

PAVAN PRAKASH GUTTI , 5th sem B.E (ECE), SDM COLLEGE OF ENGINEERING AND TECHNOLOGY  ,DHARWAD-580002
* Contact : pavan.gutti2@gmail.com



