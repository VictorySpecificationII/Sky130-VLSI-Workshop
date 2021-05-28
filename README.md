# RTL Design in Verilog using SKY130 Technology
## Workshop, 26-30th May 2021


### Project Scope

The workshop format comprises of hardware design in Verilog and follow along labs hosted by VSD-IAT on the cloud.  

The first day begins with an intro to digital design, and continues to illustrate various digital design steps such as simulation, <br/>validation by using a test bench (or stimulus file) in iverilog and logic Synthesis by using Yosys along with Sky's .lib file <br/>containing their cells and cells' features.

The second day continues with learning about Hierarchical vs Flat design, learning about flops, synchronous and asynchronous <br/>operation, alongside the techniques used in synthesis to account for phenomena such as stacked P-MOS vs stacked N-MOS.<br/> It further delves into hardware optimizations.


### Getting Started

You need one VM, preferrably running ubuntu, along with Kunal's vsdflow script which installs the necessary<br/> software stack; iVerilog for simulation, Yosys for synthesis, qRouter for routing.The workshop assumes some degree <br/>of basic familiarity with the Verilog Hardware Description Language, o if you don't know Verilog, this might not be <br/>the best place to start.

# Day 1 - Introduction to Verilog RTL Design and Synthesis

The first day covers the basics of RTL Design, Testbench, Simulation and Synthesis. The labs provide the simulator (iVerilog) <br/>and synthesis (Yosys) tools for experimentation.

## 1 - Setting up the environment.
The first step is cloning the vsdflow and sky130RTLDesignAndSynthesisWorkshop repositories from Kunal's profile, we can get to work.

![alt text](https://raw.githubusercontent.com/VictorySpecificationII/Sky130-VLSI-Workshop/master/Images/Day1/1%20-%20cloning%20repos.JPG?raw=true)


## 2 - Performing Simulations and Verification.
In order to validate a design, we need to have 

1. Written the design

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/2%20-%20good%20mux%20verilog.JPG?raw=true)



2. Written the stimulus file.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/3%20-%20good%20mux%20tb.JPG?raw=true)

The labs provide us with a multiplexer design and it's corresponding test bench.


### iVerilog

The simulator checks whether the design adheres to the specification set out by the customer.

The design will be tested using the stimulus file, and the simulator looks for changes in the input signals. As the input changes, <br/>the output is evaluated; I the event that there is no change in the input signals, there is no change in the output.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/4%20-%20iverilog%20sim%20and%20gtkwave.JPG?raw=true)

#### The Test Bench 

The stimulus file contains stimulus to be fed to the design's inputs,  and the outputs of the design directed to the stimulus observer if there is one - else they are directed to a file to view through a waveform viewer.

Important note: Only the design has I/O, the test bench does not. The design slots in the middle of the test bench.

The design and testbench are fed into iverilog sim, whichg will look for changes in the input and dump changes in the output into a .vcd file. vcd stands for value change dump.

### GTKWave

GTKWave is fed the vcd so we can see the changes in the output as the input changes.


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/5%20-%20gtkwave%20window.JPG?raw=true)

### Yosys

Yosys is the synthesizer, which converts RTL to netlists.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/6%20-%20yosys%20entry.JPG?raw=true)




Just as iverilog takes a design and its stimulus, yosys takes a design and it's library file and outputs a netlist.

A netlist is the representation of a design using the standard cells in the .lib file.



#### Synthesis Verification

In order to verify synthesis, the netlist and testbench are put through iverliog to get the vcd to feed to gtkwave. The output of <br/>the netlist and testbench combo simulation must MATCH the output of the RTL design and testbench combo simulation. The <br/>testbench remains the same for both as one is RTL design and the netlist is the RTL design realized using cells from the library file.


## 3 - Performing Logic Synthesis

RTL is written verilog, and is the behavioural representation of the specification set fourth by the customer. We need to convert<br/> RTL to digital circuit, so the question becomes how do we map one to the other?

We need to perform RTL to gate-level translation,  for the design to be converted to gates and connections between those gates<br/> to be made. The output is called the netlist.


RTL + .lib -> synthesize to digital circuti

As mentioned before, a .lib file is collection of logical modules; NOT AND OR NAND NOR etc, and contains slow, medium, <br/>and fast versions of different cells.

Below, lies an explanation as to why.

### Setup time

The time frame before the clock edge during which we need to supply the data.

Using an analogy: a train departs at 5 - you must not be at the station at 5, you must be there 10 minutes earlier so you can<br/> actually board the train.

From flip flop A to combinatorial logic block to flip flop B, the clock time must be less than the propagation delay + the time to<br/> get through the combinatorial block + the setup time.

The data must be stable for a set amount of time before the clock arrives.

Fast cells make combinatorial time smaller, to satisfy setup requirements.

Clock time dictates maximum clock frequency, the smaller the clock time, the higher the speed, the better the performance

Freq_clock = 1 / clock_time

------------------------------------------------------------------------------------------
### Hold time

Hold time is the time frame after the edge of the clock during which we don't want the data to change.


We use faster cells to meet setup times, and slower cells to meet hold times.


#### Faster vs Slower cells - A deeper look


The propagation delay is the time for the input to cause a change in the output across a logic cell e.g inverter.

A load in digital circuit is capacitance, and cell delay depends on charging rate of the capacitance.

The faster the charge / discharge, the higher the current, the lower the delay.

Wider transistors - higher current, higher area, higher power.
Narrower transistors - lower current, lower area, lower power.

Wider - faster because of lower delay
Narrower - slower because of higher delay

Overall chip speed depends on a combination of slow, medium and fast cells which contribute to it's overall area, power <br/>and current draw characteristics.


### Running Yosys

#### Steps:














-Read library file<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/7%20-%20import%20lib.JPG?raw=true)

-Read verilog design<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/8%20-%20read%20verilog.JPG?raw=true)

-Pinpoint top-level module<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/9%20-%20synth%20top.JPG?raw=true)

-Convert RTL to netlist<br/>
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/10%20-%20box%20IO%20and%20components.JPG?raw=true)

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/11%20-%20total%20inventory.JPG?raw=true)

-Show netlist<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day1/12%20-%20yosys%20logic.JPG?raw=true)


-Write converted RTL to file<br/>

#### Commands:

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib           

yosys> read_verilog good_mux.v                                                     

yosys> synth -top good_mux                                                         

yosys> abc -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib                    

yosys> show                                     


# Day 2 - PLACEHOLDER



