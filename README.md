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


RTL + .lib -> synthesize to digital circuit.

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


The propagation delay is the time for the input to cause a change in the output across a logic cell e.g inverter.<br/>

A load in digital circuit is capacitance, and cell delay depends on charging rate of the capacitance.<br/>

The faster the charge / discharge, the higher the current, the lower the delay.<br/>

Wider transistors - higher current, higher area, higher power.<br/>
Narrower transistors - lower current, lower area, lower power.<br/>

Wider - faster because of lower delay<br/>
Narrower - slower because of higher delay<br/>

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


# Day 2 - Timing libs, Hierarchical vs Flat Synthesis & Efficient FlipFlop coding styles


# 1 - Further information on .lib files
Dissecting the file name of the library file, we notice some things.

sky130_fd_sc_hd__tt_025C_1v80.lib<br/>

It highlights the:

 - process node (130nm)<br/>
 - libraries can be slow, fast or typical, tt means typical process<br/>
 - 025C is temperature<br/>
 

Putting it all together:
 

 - sky130_FounDry_StandarD cell_High Density_Typical Process_Temperature_Voltage.lib



Every library clearly shows

 - Process
 - Voltage
 - Temperature



Processes always have variations during fabrication. It's not possible to obtain exactly the same outcome every time during <br/>a process e.g if you make 100 breads, not two breads are going to colour the same way.

Variations also happen also due to voltage. By varying the voltage, behaviour also changes. Same goes for temperature.

Operating environments chips run in differ from place to place, and we want the chip to run optimally in all of them, so our libraries must reflect that fact. <br/>The cells must be modeled to accommodate for a wide range of operating conditions.

------------------------------------------------------------------------------------------------------------

A library defines :
 - technology (CMOS or otherwise)
 - power (nW)
 - delays as lookup tables 
 - voltage, (V)
 - current draw (mA)
 - resitance(kOhms)
 - time units (ns)
 - capacitance (pF)
 
   ![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/1%20-%20inside%20of%20a%20lib%20file.JPG?raw=true)


------------------------------------------------------------------------------------------------------------

It contains:
 - cells
 - different flavours of different cells
 - the various gates supported
 - different features of cells
  - leakage power for each logical combination of the cell - number of combinations is two ^ the amount of inputs
  - delays
  - area it occupies
  - power port information
  - individual pin capacitance
  - power of each pin / differentiate between power pins and data pins
  - delay of each pin
  - timing information
  - rise, fall transitions, which show the skew which affects timing
  


Below we can see the different flavours of a cell that exist in the library. This example uses the nor2_1, nor2_2, and nor2_4 gates in that order

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/6%20-%20comparison%20nor2_1.JPG?raw=true)<br/>
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/7%20-%20combination%20nor2_2.JPG?raw=true)<br/>
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/8%20-%20combination%20nor2_4.JPG?raw=true)

We can see the differences in the above properties between these 3 flavours of the same gate. They all implement the same logic.<br/> nor2_1 is the lowest power, lowest area, highest delay cell while the nor2_4 cell is the highest power, highest area, lowest delay cell. <br/>We can see that there is a range of cells to choose from.
  
  


------------------------------------------------------------------------------------------------------------


# 2 - Hierarchical Synthesis vs Flat Synthesis

## Hierarchical Synthesis

We are using multiple_modules.v as an example. It is a top level module that comprises of instantiations of lower level modules <br/>which individually implement pieces of functionality. Those individual modules are then chained together to provide<br/> a bigger piece of funtionality.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/9%20-%20top%20level%20module%20that%20instantiates%20other%20modules.JPG?raw=true)<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/10%20-%20logic%20top%20levle%20file%20implements.JPG?raw=true)<br/>




Running the following commands, we are greeted with the following output


yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib           

yosys> read_verilog multiple_modules.v                                                     

yosys> synth -top mutiple_modules                                                         

yosys> abc -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib                    

yosys> show multiple_modules 

yosys> write_verilog -noattr multiple_modules_hier.v

yosyy> !nano multiple_modules_hier.v

<br/><br/>



![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/15%20-%20yosys%20show.JPG?raw=true)<br/>



The output doesn't show the AND and the OR gates, it shows them instead as individual modules because they are instances <br/> of the submodules. Thus, hierarchies are preserved. Each module and sub module is separate. Hierarchical design abstracts modules.



![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/16%20-%20netlist.JPG?raw=true)<br/>

Looking at the verilog output, the OR gate is implemented using a NAND gate and two inverters. This is due to the stacked PMOS <br/>concept, whereby implementing it in this fashion results in poor mobility. By using a NOR gate and two inverters, we encounter <br/>PMOS stacking and therefore that approach is avoided.



------------------------------------------------------------------------------------------------------------

## Flat Synthesis

By running the commands after !nano multiple_modules_hier.v, we obtain a flat synthesis version of the design.

<br/>

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib           

yosys> read_verilog multiple_modules.v                                                     

yosys> synth -top mutiple_modules                                                         

yosys> abc -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib                    

yosys> show multiple_modules 

yosys> write_verilog -noattr multiple_modules_hier.v

yosys> !nano multiple_modules_hier.v

yosys> flatten

yosys> write_verilog -noattr multiple_modules_flat.v

yosys> !nano multiple_modules_flat.v


<br/>


The flatten command doesn't preserve hierarchies, we can see the and and or modules bare, see the individual logic gates. <br/>This is the difference between hierarchical and flattened synthesis.


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/18%20-%20flattened%20multiple%20modules.JPG?raw=true)<br/>

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/19%20-%20flattened%20yosys%20show.JPG?raw=true)<br/>



Some times we wil require module level synthesis as opposed to an entire desing. This is usually employed when we:

 - need multiple instances of same module, synthesize once and replicate as many times as you need.
   - Use required module name in synth -top command
 - employ the divide and conquer approach. 
   - If it's a big design, and the tool doesn't do a good enough job
   - We give smaller portions for it to make a better job
   - Then merge netlists at the top level to get a better design


# 3 - Flops


how to code a flop

what are we trying to do

why flops?

a combinational circuit, given a set of inputs, after a propagation delay, th output is going to change
because of delay, output is going to glitch

e.g 3 inputs

a goes 0 to 1
b goes 0 to 1
output i0 is the result of a&b


c goes 1 to 0
output y is the result of i0 | c


it happens simultaneously

when c goes low, or gate sees it

at the output y, because of the propagation delay of the and gate, the result will be shifted to the right, it won't happen at the exact same time as the inputs changing logical states

during the process of the combinatorial circuit switching, we get time intervals where if the output is not the desired value as the logical expression is still in the process of being evaluated. this is called a glitch

glitches happen.

more combi circuits, more outputs will be glitchy

daisy chaining combi circuits, outpus will never settle down and we want to avoid that, so we use flops in between kind of like buffers/storage elements

flops restrict glitches as the outputs change only on the edges of the clock, giving the circuit stability as while the output of the combi circuit glitches, the flop retains stable values for the next stages which we need for the thing to function correctly

we need to init the flops else the circuit will have garbage outputs, hence the reset or set pins on the flops

set and reset - they can be sync or async


flip flops can be triggered at a pos or neg clock edge

async reset: flop can reset irrespecctive of the edge of the clock (+/-)
sync reset: flop resets at a predetermined edge (+/-)

in the sensitivity list for a async flop, we have the clock asnd the async set/reset triggers
in the sensitivity list for a sync flop, we have the clock trigger only

for the flop with both sync and async, the if statement is only evaluated when the trigger async_reset in the sensitivity list is triggered
the else if and else are evaluated at the edge of the clock

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

simulations for flops

yosys



asyncres
d may come at any point
q changes only at the clock edge

asyncset
changes at the d pin felt at the q pin at clock edge
as long as async set is one, q stays at one, when it goes 0, q now depends on and follows the clock edge
as soon as set goes back to one, q locks at one irrespective of the clock

sync reset
sync reset goes high, q waits for the next clock edge to go low
reset and d are present, reset gets higher precedence cause of how we've coded the flop

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

read_liberty -lib <path to lib file>
read verilog <name of design>.v 
synth -top <name of design printed on the terminal>
since we are using dff flip flops use the below command so yosys knows only to look for dff's
dfflibmap -liberty <path to lib file containing dff info>
abc -liberty <path to lib> - convert logic to netlist using the spec'd lib file
show - show rsuls



