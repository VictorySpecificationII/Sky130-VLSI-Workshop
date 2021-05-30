# RTL Design in Verilog using SKY130 Technology
## Workshop, 26-30th May 2021


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/VSD/vsdbanner.png?raw=true)

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


A combinational circuit, given a set of inputs, after a propagation delay, the output is going to change.<br/> Because of the propagation delay, the output is going to glitch, it will be unstable.

e.g 3 inputs

2-Input AND gate
 - a goes 0 to 1
 - b goes 0 to 1
 - output i0 is the result of a&b


2-Input OR gate
 - c goes 1 to 0
 - output y is the result of i0 | c


Input and output swithcing happens simultaneously.

When input C goes low, the OR gate sees it.

At the same time, at the output y, because of the propagation delay of the AND gate, the result as seen in the wave form will be <br/> shifted to the right, it won't happen at the exact same time as the inputs changing logical states.

------------------------

During the process of the combinatorial circuit switching, we get time intervals where if the output is not the desired value as the <br/> logical expression is still in the process of being evaluated. This is called a glitch. Glitches happen.The more combi circuits exist, <br/>the more more outputs will be glitchy. Daisy chaining combi circuits, outputs will never settle down and we want to avoid that, <br/>so we use flops in between kind of like buffers/storage elements to hold the values (Either logical 1 or logical 0).

Flops restrict glitches as the outputs change only on the edges of the clock, giving the circuit stability as while the output of the combi <br/>  circuit glitches, the flop retains stable values for the next stages which we need for the circuit to function correctly. We need to init <br/> the flops else the circuit will have garbage outputs, hence the reset or set pins on the flops.

 - Set and Reset - they can be synchronous or asynchronous.

 - Flip-flops can be triggered at a positive or negative clock edge.

 - Async reset: flop can reset irrespecctive of the edge of the clock (+/-)
 - Sync reset: flop resets at a predetermined edge (+/-)

 - In the sensitivity list for a async flop, we have the clock asnd the async set/reset triggers.
 - In the sensitivity list for a sync flop, we have the clock trigger only.

For the flop with both sync and async reset, the if statement is only evaluated when the trigger async_reset <br/> in the sensitivity list is triggered. The else if and else are evaluated at the edge of the clock.



## Flip-Flop Simulations


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/24%20-%20asyncres%20sim.JPG?raw=true)<br/>
The above is a simulation of a flip flop with an asynchronous reset. Upon closer inspection of the waveform, we notice that d may come at any point, however q changes only at the clock edge.



![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/30%20-%20asyncres%20syncres.JPG?raw=true)<br/>
The above is a simulation of a flip flop with an asynchronous and a synchronous reset. Upon closer inspection of the waveform, <br/>we notice that changes at the d pin  arefelt at the q pin at clock edge. As long as async reset is one, q stays at one, when it goes 0,<br/> q now depends on and follows the clock edge
as soon as set goes back to one, q locks at one irrespective of the clock.



![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/26%20syncres.JPG?raw=true)<br/>

The above is a simulation of a flip flop with a synchronous reset. Upon closer inspection of the waveform, we notice that when sync <br/>reset goes high, q waits for the next clock edge to go low. Reset and d signals are present, reset gets higher precedence <br/>because of how we've coded the flop.





------

The commands for synthesizing the DFF's are the same as the other modules above, bar for the dfflibmap command which <br/> links or maps the library files that contain the features of the flops to be used.

yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib           

yosys> read_verilog dff_asyncres.v                                                     

yosys> synth -top dff_asyncres                                                         

yosys> dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib                    

yosys> show 

In the implementation of the flip flop below, the reset is an active high. Yosys has turned it into an active low and has added an inverter.


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day2/28%20-%20ansycres%20dff%20yosys%20show.JPG?raw=true)<br/>


# Day 3 - Combinatorial and Sequential Optimizations

There are two types of logic, combinational and sequential. The reason that we need to perform optimizations to the logic is<br/> to get the best design in terms of area and power savings. There exist a number of different basic and advanced <br/>optimization techniques, however we cover constant propagation as well as boolean logic optimization here.

## Constant Propagation

This is a sequential logic optimization.

For some input values, a boolean expression can be reduced to a simpler expression.

e.g AND + NOR gate, output of AND feeds into input of NOR, C is NOR input number 2 for a total of 6 MOS transistors. <br/>
		
e.g y = ~((A*B)+C)

if A = 0

Y=~((0)+C) => Y = ~C

So for A = 0 this can be reduced from a 2 gate assembly to a single inverter for a total of 2 MOS transistors.


## Sequential Constant

Imagining a D flip-flop, with it's input tied low, and a reset line connected to it. Every flop with D tied off is not a sequential<br/> constant, for the flop to become sequential constant, the q pin must always take constant value.

 - If the flop is a reset flop, it's sequential constant.
 - If the flop is a set flop, it's not sequential constant - q can be either 0 or 1 <br/>depending on set or clk pins.


## Boolean Logic Optimization

Can be performed using K-maps and Quine McKluskey, and information can be found online for them.

## Examples

Here, we will take a look at a few modules from the lab to show the optimizations.

File opt_check.v has the following structure;


module opt_check (input a , input b , output y);<br/>
	assign y = a?b:0;<br/>
endmodule<br/>


It is a 2:1 mux assigning , when a is 1, it will assign b - else it will assign 0.<br/>



  yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib<br/>
  yosys> read_verilog opt_check.v<br/>
  yosys> synth -top opt_check<br/>
  yosys> opt_clean -purge // command to perform constant propagation and optimization<br/>
  yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib<br/>
  yosys> show<br/>
  
After optimization, Yosys simplifies it down to an AND gate as seen below.
<br/>


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day3/1%20-%20opt_check.JPG?raw=true)<br/>


---------

Opt_check3 has the following structure;

module opt_check3 (input a , input b, input c , output y);<br/>
	assign y = a?(c?b:0):0;<br/>
endmodule<br/>


Example 2: opt_check3 works by assigning y depending on the value of a;
 - if a is 0 then assign 0
 -if a is one then look at c
  - if c is 1, assign b
  - else assign 0
  
  


  yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib<br/>
  yosys> read_verilog opt_check3.v<br/>
  yosys> synth -top opt_check3<br/>
  yosys> opt_clean -purge // command to perform constant propagation and optimization<br/>
  yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib<br/>
  yosys> show<br/>


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day3/3%20-%20opt_check3.JPG?raw=true)<br/>




Following the same steps for the other designs, for dff_const3, we get the following optimization.
  
  
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day3/12%20-%20synth%20dff_onst3.JPG?raw=true)<br/>


MORAL: Sequential optimizations; not every flop that has a constant at the input is getting optimized, we need to look at<br/> the set/reset connections and see if q is getting a constant value or changing.


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day3/6%20-%20multiple%20module%20opt2.JPG?raw=true)<br/>

Above is the Yosys synthesis of the multiple_modules_2 design. From this picture, it's evident that logic not related to <br/>primary output wil still be optimized, as is th case for U1, U2, and U3.


# 4 Gate Level Simulation, Blocking vs Non-Blocking Assignments, Synthesis-SImulation Mismatch

At first, we simulated RTL + stimulus. Now, we simulated netlist + stimulus.Logically, it follows that RTL and netlist <br/>are logically same, and that we ca nuse the same test bench for both.

We run GLS in order to;
 - Verify logical correctness after synthesis
 - Ensure timings are met
 

# Synthesis-Simulation Mismatch

Above, we mentioned that GLS is used to verify logical correctness and timing. Gate level verilog models can be
 - Timing aware - We can validate functionality and timing too
 - Functional - We can validate functionality only

Why do i need to verify my netlist?


Because of Synthesis-Simulation Mismatch. It can happen because of;

 - Missing sensitivity list

   - The simulator works on  activity: only when input changes output will change.<br/>
   - Always block only evaluates when inputs in it's arguments list change state.

At synthesis, the synthesizer does not look at sensitivity lists and might create wrong components<br/>
	so you end up with a simulation and synthesized netlist that don't match


## Blocking / Non-Blocking assignments


Before we dive into this, it's worth to note that both statement are always inside always blocks.


Non blocking statements
 - Executes RHS when always block is entered and assigns to LHS
 - Evaluates in parallel

Blocking statements

 - Executes sequentially, in the order its written

 - Caveat 1: order matters

e.g 
	    
	    q = q0
		q0 = d

when evaluated, we end up with 2 flops.

but 
	    q0 = q
		d = q0

when evaluated, we end up with 1 flop.


example 1 has continuity, 2 does not - THIS MATTERS.

If we use non blocking assignments,	order doesn't matter at all like it does above.

MORAL: use non blocking assignments for writing sequential circuits.

 - Caveat 2: this will cause Synthesis-Simulation Mismatch

	   reg q0;

	   always @ (*)

	      y = q0 & c;
	      q0 = a | b;

Every time we enter the always block, q0's value is equal to the value it held in the previous execution cycle. <br/>So, it will calculate and return the wrong value on every subsequent execution.
	
	

 y = q0 & c will mimic a flop. When synthesized, there will be no flop. If we switch the order;


	       q0 = a | b;
	       y = q0 & c;

then y = q0 & c will be evaluated using the this cycle's value for q0.

This is another reason why we need to run GLS on the netlist and match behaviour with expected outputs.

------------------------------------------------------

## Missing Sensitivity List Synthesis/Simulation Mismatch

This lab shows a mux that is coded to function properly, below we see the simulation waveforms.

     iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
     ./a.out
     gtkwave tb_ternary_operator_mux.vcd	

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/1%20-%20ternary%20mux%20sim.JPG?raw=true)<br/>




We perform synthesis of the design;

    yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> read_verilog ternary_operator_mux.v
    yosys> synth -top ternary_operator_mux
    yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> write_verilog -noattr ternary_operator_mux_net.v
    yosys> show
  
  
 ![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/2%20-%20ternary%20operator%20mux%20yosys.JPG?raw=true)<br/>

  Steps to do GLS:

    iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
    ./a.out
    gtkwave tb_ternary_operator_mux.vcd
    
We need to invoke the two extra files during the simulation phase.

opening gtkwave, next to the uut, we see an expand symbol. This is how we know this is the netlist simulation,<br/> there exists no such thing in RTL simulations.


 ![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/3%20-%20netlist%20ternary%20sim.JPG?raw=true)<br/>
 
The demonstration below will show the mismatch between synthesis and simulation. We use the file bad_mux.v


    iverilog bad_mux.v tb_bad_mux.v
    ./a.out
    gtkwave tb_ternary_operator_mux.vcd	

 ![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/4%20-%20bad%20mux%20sim%20rtl.JPG?raw=true)<br/>
 This is the RTL simulation.


We see that only when the select line is triggered there is a change to select, in that case only when select goes high, <br/>y takes the current value of i1.

Synthesizing and writing to file for GLS.

    yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> read_verilog bad_mux.v
    yosys> synth -top bad_mux
    yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> write_verilog -noattr bad_mux_net.v
    yosys> show
    
Performing GLS:

     iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
     ./a.out
     gtkwave tb_bad_mux.vcd
     
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/5%20-%204%20and%205%20are%20sim%20synth%20mismatch%2C%20this%20pic%20is%20netlist%20bad%20mux%2C%204%20is%20rtl%20bad%20mux.JPG?raw=true)<br/>
This is the GLS.


It is evident, by looking at the RTL sim and the GLS that they differ, in the former output y is not following i0 when select is 0. 

This is what a missing sensitivity list will do. When not sure, use always @(*), which means that the always block will be <br/>evaluated whenever any signal changes.

----------------------------------------------

## Non-Blocking vs Blocking Synthesis-Simulation Mismatch

As mentioned earlier, mismatches can occur because of gaps in logic layout.

Example:

    d = x&c
    x = a|b


A is low.<br>
B is low.<br>
X is low.<br>

and with C, output should be 0.

But: a|b is evaluated using the value it was assigned from the last round of execution, and  is ANDed with current execution round<br/> C value. That's why the result of the calculation will be wrong. X will look like a floped output in simulation.


Lab: Using blocking_caveat.v module in the labs for simulation.

    iverilog blocking_caveat.v tb_blocking_caveat.v
    ./a.out
    gtkwave tb_blocking_caveat.vcd	

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/6%20-%20blocking%20caveat%20rtl%20sim.JPG?raw=true)<br/>

This is what the RTL simulation looks like for the blocking_caveat.v module.


Synthesizing;

    yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> read_verilog blocking_caveat.v
    yosys> synth -top blocking_caveat
    yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> write_verilog -noattr blocking_caveat_net.v
    yosys> show
    
    
    
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/7%20-%20blocking%20caveat%20yosys%20synth.JPG?raw=true)<br/>
    
 

Performing GLS;

    iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
    ./a.out
    gtkwave tb_blocking_caveat_net.vcd


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day4/8%20-%20blocking%20caveat%20netlist%20sim.JPG?raw=true)<br/>

Observations;
 - In RTL Simulation; when A is low, B is low, output is low
 - In Gate Level Simulation; A is low, B is low, output is high

Why? 
 - Because it's looking at the past value

Blocking statements are something to be careful with, if you have to use it, proceed with care.


# Day 5 - If, Case, For loop, For Generate



## If statement

Much like in software programming, statements are also used in creative ways in Verilog to describe hardware. <br>The most basic one is the If/Else statement. Various combinations are highlighted below;


    if(expression)
	     single statement


    if (expression)
	  begin
		multiple statements
	  end
    else (expression)
	   begin
		multiple statements
	   end


    if(expression)
		single statement
    else if (expression)
	   begin
		multiple statements
	   end else
	    single statement


In Verilog, incorrect use of statements can have unintended consequences. One of those consqeuences is the incomplete if.


An incomplere if is any if statement that at the end does not have a pure else clause.

An incomplete if will cause inferred latches in the design, which is dangerous.

When you write code think of the hardware it will translate to.

In a combinational circuit we cannot have inferred latches. 



## Case Statement


    case(signal)
        <case1> = begin
 		     	...
 			     end
        <case2> = begin
 		          ...
 			      end
        default = begin
                  ...
                  end


Caveats with case:

 - Incomplete case:
     - Just like if, it can cause inferred latches in design -> dangerous
     - Solution: code a default case.

 - Partial assignments in case

    - A partial assignment is when:
    
    

    case(signal)
        <case1> = begin
 		     	x = a;
 		     	y = b;
 			     end
        <case2> = begin
 		          x  =c;
 			      end
        default = begin
                  x = d;
                  y = b;
                  end





We never specified what y is in case 2; that's a partial assignment.
By doing that we created an inferred latch.

MORAL: assign all the outputs in all the segments of case.


 - Overlapping cases:
 
       case(signal)
         2'b00 = begin
 		      	...
 		 	    end
         2'b01 = begin
 		     	...
 			    end
         2'b10 = begin
 		     	...
 			    end
         2'b1 = begin
 		     	...
 			    end
        endcase


Cases 3 and 4 match, - causes unpredictable output.

Remember: with case statements, it checks every case even if it finds a match.

Avoid matching cases.

It is not a problem with if because it exits at a matched condition.


-----------------------------


### Lab: Incomplete if


The following snippet is taken from the labs and describes an incomplete if situation;

     module incomp_if (input i0 , input i1 , input i2 , output reg y);
       always @ (*)
       begin
	     if(i0)
		    y <= i1;
       end
     endmodule



 	
 - An if always translates to a mux



This translates into a mux with 
 - i0 as the select signal 
 - i1 as one of the inputs
 - y as the output
 - the second input is connected to from the output
 
 
It essentially is a D latch.


Performing an RTL simulation:

     iverilog incomp_if.v tb_incomp_if.v
     ./a.out
     gtkwave tb_incomp_if.vcd
     
     

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/1%20-%20incomp%20if%20rtl%20sim.JPG?raw=true)

     
We can see that when our i0 goes low, y latches on to something. There is no change in y whenever i0 is low.<br/>
y depends on the value of i1 as i0 goes low, so if i1 is 1 when i0 goes low, the output latches to 1. <br/>The same happens when i1 is 0.

This is what happens with an incomplete if.


Synthesizing the design;



    yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> read_verilog incomp_if.v
    yosys> synth -top tb_incomp_if
    yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> show


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/2%20-%20incomp%20if%20yosys.JPG?raw=true)


We tried coding it as a mux but the incomplete if caveat caused Yosys to synthesize it using a latch, this is an inferred latch.


Below is the RTL simulation and Yosys synthesis using the incomp_if2.v file.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/3%20-%20incomp%20if2%20rtl.JPG?raw=true)

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/4%20-%20incomp%20if2%20yosys.JPG?raw=true)

We see that without the else clause, it causes Yosys to synthesize using a latch.


-------------------------------

### Lab: Incomplete Case

Below, the code snippet describes an incomplete case;



    module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
     always @ (*)
      begin
	   case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	   endcase
      end
    endmodule


There is no default statement coded so it's going to latch.

Just like an if statement, a case statement also translates into a mux.


Performing an RTL simulation;

    iverilog incomp_case.v tb_incomp_case.v
    ./a.out
    gtkwave tb_incomp_case.vcd

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/5%20-%20incomp%20case%201%20rtl%20sim.JPG?raw=true)



This design has 2 bit select line.

 - When sel 00, y folows i0
 - When sel 01, follows i1
 - When sel 10 or 11
   - y latches to the value it had before

Synthesizing the design;


     yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     yosys> read_verilog incomp_case.v
     yosys> synth -top tb_incomp_case
     yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     yosys> show

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/6%20-%20incomp%20case%20yosys.JPG?raw=true)


We can see the latch at the end.

<br>

### Lab: Complete Case

Below we see the same implementation using a default case; The latch disappears.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/7%20-%20comp%20case%20rtl.JPG?raw=true)

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/8%20-%20comp%20case%20yosys.JPG?raw=true)






---------------------------------------------------


### Lab: Partial assignment case


in case of partial assignment, we will encounter a latch as well. The synthesizer doesn't know what to do, so it defaults to a latch.

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/9%20-%20partial%20case%20yosys.JPG?raw=true)



-------------------------------------------------


### Lab: Overlapping cases


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/10%20bad%20case%20-%20rtl.JPG?raw=true)



Here, we can see that whenever select is equal to 2'b11, the tool becomes confused.

Synthesizing the design


     yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     yosys> read_verilog incomp_case.v
     yosys> synth -top tb_incomp_case
     yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
     yosys> write_verilog -noattr bad_case_net.v


Performing GLS:

    iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v bad_case_net.v tb_bad_case.v
    ./a.out
    gtkwave tb_bad_case.vcd
    

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/11%20-%20bad%20case%20gls.JPG?raw=true)



## For Loops


There are two For loop implementations, which offer distinct functionalities.

     for loop

The plain for loop is used for evaluating expressions.

     generate   
  	   for loop 

The above for loop is always outside the always block, and should not/cannot be used inside an always block.<br>It's purpose is to instantiate hardware.




Below we have a mux that has been coded with a for loop:

    module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
     wire [3:0] i_int;
     assign i_int = {i3,i2,i1,i0};
     integer k;
      always @ (*)
       begin
        for(k = 0; k < 4; k=k+1) begin
	     if(k == sel)
		  y = i_int[k];
         end
        end
     endmodule


This is a 4:1 mux, and i is a 4 bit bus. Select is a 2 bit input, and k is an integer.

every time a signal changes, if the integer held in k matches the two bit input in select, 

whatever the value of select is, the corresponding input is selected as an output.



Running an RTL Simulation;


    iverilog mux_generate.v tb_mux_generate.v
    ./a.out
    gtkwave tb_mux_generate.vcd


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/12%20-%20mux_gen%20rtl%20sim.JPG?raw=true)



 - Why write it this way? 
 - Why not write it with a case statement?

It scales very badly; for a 256 mux you need to type out 256 cases, BY HAND. Whereas, with that for loop, you only need to change 1 number.

    yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> read_verilog mux_generate.v
    yosys> synth -top mux_generate
    yosys> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    yosys> show
    yosys> write_verilog -noattr mux_generate_net.v


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/13%20-%20mux%20gen%20yosys.JPG?raw=true)

Performing GLS:

     iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky_fd_sc_hd.v mux_generate_net.v tb_mux_generate.v
     ./a.out
     gtkwave tb_mux_generate.vcd
     
     
![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/14%20-%20mux%20generate%20gls.JPG?raw=true)
------------------------------------------------------------

	
### Lab - Demux using a case statement

The function of a demultiplexer:

 - All outputs are 0
 - One input
 - Depending on the select, one of the outputs will follow input



     module demux_case (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
     reg [7:0]y_int;
     assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
     integer k;
     always @ (*)
      begin
      y_int = 8'b0;
	    case(sel)
		  3'b000 : y_int[0] = i;
		  3'b001 : y_int[1] = i;
		  3'b010 : y_int[2] = i;
		  3'b011 : y_int[3] = i;
		  3'b100 : y_int[4] = i;
		  3'b101 : y_int[5] = i;
		  3'b110 : y_int[6] = i;
		  3'b111 : y_int[7] = i;
	    endcase
      end
    endmodule


Important to note: all outputs are assigned 0 to avoid inferred latches.

Below is the RTL Simulation:

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/15%20-%20demux%20case%20rtl.JPG?raw=true)





Below we have a demux coded with a for loop
Important to note: all outputs are assigned 0 to avoid inferred latches.


    module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 ,     output o7 , input [2:0] sel  , input i);
    reg [7:0]y_int;
    assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
    integer k;
    always @ (*)
     begin
      y_int = 8'b0;
      for(k = 0; k < 8; k++) begin
	    if(k == sel)
		 y_int[k] = i;
        end
       end
     endmodule


It's corresponding RTL simulation;

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/16%20-%20demux%20generate%20rtl.JPG?raw=true)



Comparing these two pictures, we see that the functionality is identical. It further goes to highlight the scalability issue between the two approaches.

-----------------------------------

### Lab: Ripple Carry Adder - For Generate



If we need to add two numbers, we use a RCA.

A RCA needs x full adders in order to process x bit numbers. These full adders are chained together.

By using the case statement, we will have to instantiate and connect x instances by hand,  <br/>which is not efficient, so we use For Generate loops.By instantiating 1 full adder, we can replicate <br/>
as many times as we need.




    module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
     wire [7:0] int_sum;
     wire [7:0]int_co;

     genvar i;
     generate
	  for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	  end
     endgenerate
        fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));
        assign sum[7:0] = int_sum;
        assign sum[8] = int_co[7];
    endmodule



The above describes a 8-bit RCA.

In the for block inside the generate enclosure, the way the index is coded causes the instances to connect to each other. It's a more elegant design.


Trying to simulate the RCA with its test bench will cause iVerilog to complain, as the full adder instantiation is in a different file, so we have to call it too, like we did with the primitives earlier.


    iverilog fa.v rca.v tb_rca.v
    ./a.out
    ./gtkwave 


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/17%20-%20ripple%20carry%20adder%20rtl%20sim.JPG?raw=true)



Synthesizing it;

    read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    read_verilog rca.v fa.v
    synth -top rca
    abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    write_verilog rca_net.v


Yosys synthesis of the RCA:


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/18%20-%20rca%20yosys.JPG?raw=true)



Yosys Synthesis of the Full Adder:

![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/19%20-%20fa%20yosys.JPG?raw=true)



Performing GLS:

    iverilog fa.v rca.v
    ./a.out
    gtkwave tb_rca.vcd


![alt text](https://github.com/VictorySpecificationII/Sky130-VLSI-Workshop/blob/master/Images/Day5/20%20-%20rca%20net%20gls.JPG?raw=true)


--------


# Acknowledgements


 - Kunal Ghosh
 - VSD-IAT
 
