# RTL Design and Synthesis in Verilog using Sky130 Technology <!-- omit in toc -->
<img width="900" alt="VSD_Workshop_Detail" src="https://user-images.githubusercontent.com/93824690/166194735-0f0b80b3-6f14-4d76-8929-c3052b3aa3df.png">



##Table of Contents
- [Introduction](#-introduction)
- [1. Day 1 - Introduction to Verilog RTL design and Synthesis](#1-day-1---introduction-to-verilog-rtl-design-and-synthesis)
  - [1.1. Introduction to open source simulator iverilog](#11-introduction-to-open-source-simulator-iverilog)
  - [1.2. Labs using iverilog and gtkwave](#12-labs-using-iverilog-and-gtkwave)
  - [1.3. Introduction to Yosys and Logic Synthesis](#13-introduction-to-Yosys-and-Logic-Synthesis)
  - [1.4. Labs using Yosys and Sky130 PDKs](#14-labs-using-Yosys-and-Sky130-PDKs)
 - [2. Day 2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles](#2-day-2---timing-libs-hierarchical-vs-flat-synthesis-and-efficient-flop-coding-styles)
  - [2.1. Introduction to Timing .libs](#21-introduction-to-timing-.libs)
  - [2.2. Hierarchial synthesis vs Flat synthesis](#22-hierarchial-synthesis-vs-flat-synthesis)
  - [2.3. Various Flop Coding Styles and optimization](#23-various-flop-coding-styles-and-optimization)
- [3. Day 3 - Combinational and Sequential Optimizations](#3-day-3---combinational-and-sequential-optimizations)
  - [3.1. Introduction to Optimizations](#31-introduction-to-optimizations)
  - [3.2. Combinational logic Optimizations](#32-combinational-logic-optimizations)
  - [3.3. Sequential logic Optimizations](#33-sequential-logic-optimizations)
- [4. Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#4-day-4---GLS,-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
  - [4.1. GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements](#41-GLS,-Synthesis-Simulation-mismatch-and-Blocking/Non-blocking-statements)
  - [4.2. Labs on GLS and Synthesis-Simulation Mismatch](#42-Labs-on-GLS-and-Synthesis-Simulation-Mismatch)
  - [4.3. Labs on synth-sim mismatch for blocking statement](#43-Labs-on-synth-sim-mismatch-for-blocking-statement)
- [5. Day 5 - Optimization in synthesis](#1-day-1---optimization-in-synthesis)
  - [5.1. If Case constructs](#51-if-Case-constructs)
  - [5.2. Labs on "Incomplete If Case"](#52-labs-on-"incomplete-if-case")
  - [5.3. Labs on "Incomplete overlapping Case"](#53-labs-on-"incomplete-overlapping-case")
  - [5.4. for loop and for generate"](#54-for-loop-and-for-generate")
  - [5.5. Labs on "for loop" and "for generate"](#55-labs-on-"for-loop"-and-"for-generate")
  - 
# Introduction
The report is based on 5-day RTL Design and Synthesis in Verilog using the SKY130 Technolog workshop facilitated by VSD on using open source tools involving **iVerilog, GTKWave, Yosys** with **Sky130 technology**.  
This workshop introduces to the digital logic design using Verilog HDL. Validating the functionality of the design using Functional Simulation. Writing Test Benches to validate the functionality of the RTL design .Logic synthesis of the Functional RTL Code. Gate Level Simulation of the Synthesized Netlist.

**SKY130** is the hardware industry's first open-source process design kit (PDK) released by SkyWater Technology Foundry in collaboration with Google giving all hardware design experts and aficionados, a worldwide access to their IP functions and open source ASICs. 

## 1. Day 1 - Introduction to Verilog RTL design and Synthesis
### 1.1. Introduction to open source simulator iverilog
In the digital circuit design, **register-transfer level (RTL)** is a design abstraction which models a synchronous digital circuit in terms of the data flow between hardware register, and the logical operations performed on those signals. RTL abstraction is used in HDL to create high-level representations of a circuit, from which lower-level representations and ultimately actual wiring can be derived.  

**Simulator**: It is a tool which is used for checking the design. In this workshop we are using **iverilog** tool.**Simulation** is the process of creating models that mimic the behavior of the device you are designing (simulation models) and creating models to exercise the device (test benches).
**RTL Design**: It consists of an actual verilog code / a set of verilog codes that have the functionality to meet the required design specifications of the circuit.

**Test Bench**: It is the setup to apply stimulus(test vectors) to design to checks its functionality.

#### HOW SIMULATOR WORKS 
**Simulator** looks for changes on input signals and based on that output is evaluated.

<img width="500" alt="test bench" src="https://user-images.githubusercontent.com/93824690/166195956-3208f1c5-a7f8-405a-bdf7-08c2ae92ac4b.png">


**Design** may have 1 or more primary inputs and primary outputs but **TB** doesn't have.)

 #### SIMULATION FLOW
<img width="500" alt="iverilog based" src="https://user-images.githubusercontent.com/93824690/166142840-4d8a8377-526c-444e-9498-dc76068046fc.png">

**Simulator** continuously checks for changes in the input. If there is an input change, the output is evaluated; else the simulator will never evaluate the output.

### 1.2. Labs using iverilog & gtkwave
 
#### ENVIRONMENT SETUP

```
//create a directory
$ mkdir VLSI 
//Git Clone vsdflow. 
$ git clone https://github.com/kunalg123/vsdflow.git
//Git Clone sky130RTLDesignAndSynthesisWorkshop. 
$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
<img width="641" alt="Screenshot (130)" src="https://user-images.githubusercontent.com/93824690/166142874-66d2c8f4-0fc5-4a82-91ae-b3821f08f56e.png"> 
 
**sky130RTLDesignAndSynthesisWorkshop** Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes.

_We are given a default set of files and libraries shown below to work on using the practical lab instance._

<img width="641" alt="Screenshot (134)" src="https://user-images.githubusercontent.com/93824690/166143655-b00175e1-864b-4aac-8c3c-a4f72e388351.png">

### Simulation using iverilog simulator - 2:1 multiplexer rtl design

#### VERILOG FILE OF A SIMPLE 2:1 MUX

<img width="641" alt="Screenshot (135)" src="https://user-images.githubusercontent.com/93824690/166143401-cb52b623-5095-45f4-b880-59b88e4c4bca.png">


#### GTKWAVE Analysis

<img src="https://user-images.githubusercontent.com/93824690/166143969-6e8fb91c-fc09-4a8b-b779-5b45b834888c.png" width="641">


#### Access Module Files
```
$ gvim tb_good_mux.v -o good_mux.v 
```

<img width="400" alt="Screen Shot 2021-09-02 at 12 23 28 AM" src="https://user-images.githubusercontent.com/89927660/131786618-c6d4663f-5375-48dc-aa16-46b70e6797da.png">

### 1.3. Introduction to Yosys & Logic Synthesis

**Synthesizer** is a tool for converting the **RTL** to Netlist and here we are using the **Yosys** Synthesizer.
#### Yosys SETUP
<img width="500" alt="Yosys" src="https://user-images.githubusercontent.com/93824690/166144581-f9888922-5b97-467b-bac8-42138d4c8a7e.png">



<img width="500" alt="verify the synthesis" src="https://user-images.githubusercontent.com/93824690/166144585-f308505e-2f1a-468f-aff4-673800445259.png">

#### Logic Synthesis

RTL Design - behavioral representation in HDL form for the required specification.

 **Synthesis** - RTL to Gate level translation.
 The design is converted int gates and connections are made. This given outas a file called **netlist**.

>_.lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._

#### Faster cells and Slower Cells

A cell delay in the digital logic circuit depends on the load of the circuit which here is Capacitance.

Faster the charging / discharging of the capacitance --> Lesser is the Cell Delay

Inorder to charge/discharge the capacitance faster, we use wider transistors that can source more current. This will help us reduce the cell delay but at the same time, wider transistors consumer more power and area. Similarly, using narrower transistors help in reduced area and power but the circuit will have a higher cell delay. Hence, we have to compromise on area and power if we are to design a circuit with low cell delay.

#### Constraints

A Constraint is a guidance file given to a synthesizer inorder to enable an optimum implementation of the logic circuit by selecting the appropriate flavour of cells (fast or slow).

### 1.4. Labs using Yosys and Sky130 PDKs

#### Steps for Design Synthesis

<img width="500" alt="Screen Shot 2021-09-02 at 12 16 52 AM" src="https://user-images.githubusercontent.com/89927660/131785975-bbc0c874-8b81-4f29-b892-3b279c7dbc6c.png">

#### Inference from Synthesis and Execution of netlist generation

<img width="641" alt="Screenshot (151)" src="https://user-images.githubusercontent.com/93824690/166145643-8430fe11-9020-44dc-b649-194343b4f955.png">

#### Synthesis by using show command

<img width="500" alt="Screenshot (153)" src="https://user-images.githubusercontent.com/93824690/166145665-dfcf9e19-d920-4819-bdcd-793b6209da5c.png">

#### Writing Netlist

<img width="641" alt="Screenshot (157)" src="https://user-images.githubusercontent.com/93824690/166146410-42e93e44-0fba-4540-a819-95afff4e1b8b.png">

## 2. Day 2 - Timing Libs, Hierarchial Vs Flat Synthesis and Efficient Flop Coding Styles
### 2.1. Introduction to timing labs
```
__Command to open the libary file
$ gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
__To shut off the background colors/ syntax off:
: syn off
__To enable the line numbers
: se nu
```
#### Library file

<img width="641" alt="Screenshot (150)" src="https://user-images.githubusercontent.com/93824690/166147361-8f538de7-24d2-410e-81c2-832710298c5a.png">

#### Contents

For a design to work, there are three important parameters that determines how the Silicon works: Process (Variations due to Fabrications), Voltage (Changes in the behavior of the circuit) and Temperature (Sensitivity of semiconductors). Libraries are characterized to model these variations. 

<img width="300" alt="Screenshot (162)" src="https://user-images.githubusercontent.com/93824690/166147849-14fa3eaf-0ca9-440c-bbe8-7b52f6cd682e.png">

#### Various Flavours of AND Cell

<img width="750" alt="Screenshot (163)" src="https://user-images.githubusercontent.com/93824690/166147855-692033f0-08e1-465f-98b6-5a9fba6a3eb0.png">


### 2.2. Hierarchial synthesis vs Flat synthesis 

#### Hierarchial synthesis  
````
_Opening the file used for this experiment
$ gvim multiple_modules.v
_Invoke Yosys
$ yosys
_Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Read Design
$ read_verilog multiple_modules.v
_Synthesize Design
$ synth -top multiple_modules
_Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__t_025C_1v80.lib
_Realizing Graphical Version of Logic for multiple modules
$ show multiple_modules
_Writing the netlist in a crisp manner 
$ write_verilog -noattr multiple_modules_hier.v
$ !gvim multiple_modules_hier.v
````
**Multiple Modules:** - 2 SubModules
**Staistics of Multiple Modules**

<img width="641" alt="Screenshot (165)" src="https://user-images.githubusercontent.com/93824690/166205646-1597fd0a-12e7-4244-b0a7-875b36e8366b.png">

**Realization of the Logic**

<img width="500" alt="Screen Shot 2021-09-02 at 5 12 46 PM" src="https://user-images.githubusercontent.com/89927660/131923051-5d882430-fa4a-4b0d-8b70-0857c58f9b34.png">

**Netlist file**

<img width="641" alt="Screenshot (168)" src="https://user-images.githubusercontent.com/93824690/166205875-e11616c0-01c5-4aee-a5a8-061646e8bbb7.png">

#### Flat synthesis  

```
_To flatten the netlist
$ flatten
_Writing the netlist in a crisp manner and to view it
$ write_verilog -noattr multiple_modules_flat.v
$ !gvim multiple_modules_flat.v
```
**Realization of the Logic**

<img width="1000" alt="Screen Shot 2021-09-02 at 6 14 16 PM" src="https://user-images.githubusercontent.com/89927660/131927662-d25c4d37-c0c1-41a7-ab14-9399840eb3ee.png">
  
 
**Netlist file**

<img width="641" alt="Screenshot (169)" src="https://user-images.githubusercontent.com/93824690/166206713-c95c9f66-34f0-408d-916c-47081c326872.png">


#### SUB MODULE LEVEL SYNTHESIS

Sub-module level synthesis is preferred when there are multiple instances of same module. Sythesizing the same module over several times may not be advantageous with respect to time. Instead, synthsis can be performed for one module, its netlist can be replicated and then stitched together in the top module. This is also used particulary in massive designs using divide and conquer method. 

**Statistics of Sub-module**

<img width="300" alt="Screen Shot 2021-09-02 at 9 11 28 PM" src="https://user-images.githubusercontent.com/89927660/131940332-d8272cc3-affb-471d-9dd0-1ad951d86c22.png">

**Graphical Realization of the Logic**

<img width="500" alt="Screen Shot 2021-09-02 at 9 12 18 PM" src="https://user-images.githubusercontent.com/89927660/131940277-9fc3e2fe-b185-4d91-9b2d-86e54c1d1768.png">

**NetList File of Sub-module**

<img width="300" alt="Screen Shot 2021-09-02 at 9 13 33 PM" src="https://user-images.githubusercontent.com/89927660/131940384-c0bf6a0a-a73c-4c99-95a4-84a7a654e774.png">


### 2.3. Various Flop coding styles and optimization
In a digital design, when an input signal changes state, the output changes after a propogation delay. All logic gates add some delay to singals. These delays cause expected and unwanted transitions in the output, called as _Glitches_ where the output value is momentarily different from the expected value. An increased delay in one path can cause glitch when those signals are combined at the output gate. In short, more combinational circuits lead to more glitchy outputs that will not settle down with the output value. 

#### Flip flop overview
A D flip-flop is a sequential element that follows the input pin d at the clock's given edge. D flip-flop is a fundamental component in digital logic circuits.
There are two types of D Flip-Flops being implemented: Rising-Edge D Flip Flop and Falling-Edge D Flip Flop.

<img width="400" alt="fe verilog-d-flip-flop" src="https://user-images.githubusercontent.com/93824690/166153680-a44e9aa1-4056-4cfe-8a09-a096e3da9dc1.png">

Every flop element needs an initial state, else the combinational circuit will evaluate to a garbage value. In order to achieve this, there are control pins in the flop namely: Set and Reset which can either be Synchronous or Asynchronous. 

#### _Asynchronous Reset/Set:_

<img width="700" alt="Screen Shot 2021-09-02 at 10 11 42 PM" src="https://user-images.githubusercontent.com/89927660/131946717-7cdd0aeb-369a-4ade-b6d2-b9b7fc51b13e.png">

<img width="700" alt="Screen Shot 2021-09-02 at 10 16 12 PM" src="https://user-images.githubusercontent.com/89927660/131947092-dd26c7ca-25c2-48d0-9841-d9e0ad5d5a66.png">


>_ Here, always block gets evaluated when there is a change in the clock or change in the set/reset.The circuit is sensitive to positive edge of the clock. Upon the signal going low/high depending on reset or set control, singal q line goes changes respectively. Hence, it does not wait for the positive edge of the clock and happens irrespective of the clock_.

#### _Synchronous Reset:_

<img width="641" alt="Screen Shot 2021-09-02 at 10 19 42 PM" src="https://user-images.githubusercontent.com/89927660/131946706-2222f473-89bc-4a2a-85df-3ffb0c01e77e.png">


#### _Both Synchronous and Asynchronous Reset:_

<img width="700" alt="Screen Shot 2021-09-02 at 10 28 38 PM" src="https://user-images.githubusercontent.com/89927660/131946998-d79712a6-2c72-44ae-95c2-596372b3365c.png">


#### FLIP FLOP SIMULATION

```
#Steps Followed for analysing Asynchronous behavior:
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog dff_asyncres.v tb_dff_asyncres.v 
//List so as to ensure that it has been added to the simulator
$ ls
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_dff_asyncres.vcd
```
**GTK WAVE OF ASYNCHRONOUS RESET**

<img width="641" alt="Screenshot (181)" src="https://user-images.githubusercontent.com/93824690/166207949-1b806556-0aa2-4671-b1a0-2d16b55fbbdc.png">

#### FLIP FLOP SYNTHESIS

```
_Invoke Yosys
$ yosys
_Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Read Design
$ read_verilog dff_asyncres.v
_Synthesize Design - this controls which module to synthesize
$ synth -top dff_asyncres
_There will be a separate flop library under a standard library
_But here we point back to the same library and tool looks only for DFF instead of all cells
$ dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Realizing Graphical Version of Logic for single modules
$ show 
_Writing the netlist in a crisp manner 
$ write_verilog -noattr dff_asyncres_ff.v
$ !gvim dff_asyncres_ff.v
```
**Statistics of D FLipflop with Asynchronous Reset**

<img width="400" alt="Screenshot (185)" src="https://user-images.githubusercontent.com/93824690/166207965-d6027bd0-b4be-4bd9-9e98-fff90892aae3.png">

**Realization of Logic**

<img width="700" alt="Screenshot (187)" src="https://user-images.githubusercontent.com/93824690/166208007-645ed477-f0cf-4af2-a6dc-0c692498d26d.png">

**Statistics of D FLipflop with Asynchronous set**

<img width="400" alt="Screenshot (188)" src="https://user-images.githubusercontent.com/93824690/166207987-427e675c-d2d2-4677-949f-5eb27b18ce03.png">

**Realization of Logic**

<img width="700" alt="Screenshot (189)" src="https://user-images.githubusercontent.com/93824690/166208020-8054aabb-5e98-424f-87cb-5ea69d9962ea.png">

**Statistics of D FLipflop with Synchronous Reset**

<img width="400" alt="Screenshot (190)" src="https://user-images.githubusercontent.com/93824690/166209133-d0fef1ae-39d3-43e3-8ab3-25d746b881a1.png">

**Realization of Logic**

<img width="700" alt="Screenshot (191)" src="https://user-images.githubusercontent.com/93824690/166209147-347c8784-a4e8-44fe-bf08-19c9b9b6e3c6.png">

#### Interesting Optimizations
```
modules used are opened using the command
$ gvim mult_*.v -o
_Invoke Yosys
$ yosys
_Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Read Design
$ read_verilog mult_2.v
_Synthesize Design - this controls which module to synthesize
$ synth -top mul2
_Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
_Realizing Graphical Version of Logic for single modules
$ show 
_Writing the netlist in a crisp manner 
$ write_verilog -noattr mult_2.v
$ !gvim mult_2.v
```
## (i) mult_2.v 

**_Expected Logic_**

<img width="400" alt="Screenshot (195)" src="https://user-images.githubusercontent.com/93824690/166213003-2cf85b52-fd17-4f12-a404-b86da6d559fb.png">

**_Statistics & abc command return due to absence of standard cell library_**

<img width="400" alt="Screenshot (196)" src="https://user-images.githubusercontent.com/93824690/166213036-a483bd0c-8a76-4701-846c-dbaa416c3d10.png">

 ##### No hardware requirements - No # of memories, memory bites, processes and cells. Number of cells inferred is 0.
 
 **_NetList File of Sub-module_**
 
<img width="200" alt="Screenshot (198)" src="https://user-images.githubusercontent.com/93824690/166213102-cc337c8f-74b0-475e-a622-81a3f3966cec.png">

 **_Realization of Logic_**
 
<img width="400" alt="Screenshot (197)" src="https://user-images.githubusercontent.com/93824690/166213092-96fdb5cc-2bd2-468b-a347-df2fc90980c5.png">

## (ii) mult_8.v

**_Expected Logic_**

<img width="400" alt="Screenshot (194)" src="https://user-images.githubusercontent.com/93824690/166212974-d283029d-43ec-45be-bb02-6769eb3331f9.png">
<img width="300" alt="Screen Shot 2021-09-04 at 4 19 20 AM" src="https://user-images.githubusercontent.com/89927660/132089537-ea9225f5-00ed-462f-b61e-208a3ae8d25e.png">
**_Statistics _**

<img width="400" alt="Screenshot (199)" src="https://user-images.githubusercontent.com/93824690/166213113-155dc525-71d2-48b5-8608-3f5351254cc5.png">
 
 **_NetList File of Sub-module_**
 
<img width="200" alt="Screenshot (202)" src="https://user-images.githubusercontent.com/93824690/166213150-51ca63d3-365e-48f5-b0db-c95ccc0ef7df.png">

 **_Realization of Logic_**
 
<img width="400" alt="Screenshot (200)" src="https://user-images.githubusercontent.com/93824690/166213140-c617aa85-8022-4f84-952b-bfa8970438e2.png">

----------------------------------------------------------------------------------------------------------------------------------------------------------

## 3. Day 3 - Combinational and Sequential Optimizations
**Logic Circuits**
Combinational circuits are defined as the time independent circuits which do not depends upon previous inputs to generate any output are termed as combinational circuits. Sequential circuits are those which are dependent on clock cycles and depends on present as well as past inputs to generate any output.

### 3.1. Introduction to Logic Optimizations

#### Combinational Logic Optimization
**Why do we need Combinational Logic Optimizations?**

-[Primarily to squeeze the logic to get the most optimized design] (#Primarily-to-squeeze-the-logic-to-get-the-most-optimized-design)
  -[An optimized design results in comprehensive Area and Power saving] (#An-optimized-design-results-in-comprehensive-Area-and-Power-saving)

#### Types of Combinational Optimizations

* Constant Propagation 
	* Direct Optimization technique
* Boolean Logic Optimization.
	* Karnaugh map
	* Quine Mckluskey

#### CONSTANT PROPAGATION

In Constant propagation techniques, inputs that are no way related or affecting the changes in the output are ignored/optimized to simplify the combination logic thereby saving area and power usage by those input pins.
```
Y =((AB)+ C)'
If A = 0
Y =((0)+ C)' = C'
```
#### BOOLEAN LOGIC OPTIMIZATION

Boolean logic optimization is nothing simplifying a complex boolean expression into a simplified expression by utilizing the laws of boolean logic algebra.
```
assign y = a?(b?c:(c?a:0)):(!c)
```
above is simplified as
```
y = a'c' + a(bc + b'ca) 
y = a'c' + abc + ab'c 
y = a'c' + ac(b+b') 
y = a'c' + ac
y = a xor c
```
### Sequential Logic Optimization

#### Types of Sequential Optimizations

* Basic Technique
	* Sequential Constant Propagation
* Advanced Technique
	* State Optimization
	* Retiming
	* Sequential Logic cloning(Floorplan aware synthesis)
	
#### COMBINATIONAL LOGIC OPTIMIZATION  
```
//to view all optimization files
$ ls *opt_check*
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog opt_check.v
//Synthesize Design - this controls which module to synthesize
$ synth -top opt_check
//To perform constant propogation optimization
$ opt_clean -purge
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```
<img width="400" alt="Screenshot (203)" src="https://user-images.githubusercontent.com/93824690/166223707-f4d4d1ca-927c-4be8-8cea-e2c7de993af6.png">

##### (i)opt_check.v
**_Expected logic from verilog file_**

<img width="400" alt="13" src="https://user-images.githubusercontent.com/93824690/166227292-97c49c8d-0b68-4a8a-93cd-387c93d6eaf6.png">

>_value of y depends on a, y = ab._

**_Command for constant propogation method_**

<img width="400" alt="2" src="https://user-images.githubusercontent.com/93824690/166227551-6acaad42-bf9b-43be-801a-1d4c5d97d48d.png">

**_Realization of the Logic_**

<img width="641" alt="Screenshot (206)" src="https://user-images.githubusercontent.com/93824690/166224940-a557c7c1-5e43-4dc1-8776-136b8c21aa2a.png">

>_optimized graphical realization thus shows a 2-input AND gate being implemented._

#### (ii)opt_check2.v
**_Expected logic from verilog file_**

<img width="400" alt="opt check 2 mo" src="https://user-images.githubusercontent.com/93824690/166227941-e7372172-6854-4794-9e2a-1594d6024e18.png">

>_value of y depends on a, y = a+b._

**_Realization of the Logic_**

<img width="641" alt="Screenshot (208)" src="https://user-images.githubusercontent.com/93824690/166224954-5bc8b24c-a42a-456a-a1c8-e6d77d127ab9.png">

>_optimized graphical realization thus shows 2-input OR gate being implemented. Although OR gate can be realized using NOR, it can lead to having stacked PMOS configuration which is not a design recommendation. So the OR gate is realized using NAND and NOT gates (which has stacked NMOS configuration)._

#### (iii)opt_check3.v
**_Expected logic from verilog file_**

<img width="400" alt="opt check 3 mo" src="https://user-images.githubusercontent.com/93824690/166228053-f18837e0-0639-432c-8abe-3e3e59ad62f1.png">

>_value of y depends on a, y = abc._

**_Realization of the Logic_**
<img width="641" alt="Screenshot (209)" src="https://user-images.githubusercontent.com/93824690/166224962-c2c38237-33f0-400e-afdf-867eddb8235b.png">

>_optimized graphical realization thus shows 3-input AND gate being implemented._

#### (iv)opt_check4.v
**_Expected logic from verilog file_**

<img width="400" alt="opt check 4 mo" src="https://user-images.githubusercontent.com/93824690/166228431-140351df-3af6-4664-bf41-1cb1c04a12ce.png">

>_The value of y depends on a, y = a'c + ac_

**_Realization of the Logic_**

<img width="641" alt="opt check 4" src="https://user-images.githubusercontent.com/93824690/166228436-ed297d9d-c379-41c1-add5-913614b9540e.png">

>_optimized graphical realization thus shows A XNOR C gate being implemented._


#### SEQUENTIAL LOGIC OPTIMIZATION  
```
//To view all optimization files
$ ls *df*const*
//To open multiple files 
$ dff_const1.v -o dff_const2.v
//Performing Simulation
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog dff_const1.v tb_dff_const1.v 
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_dff_const1.vcd
//Performing Synthesis
//Invoke Yosys 
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog dff_const1.v
//Synthesize Design - this controls which module to synthesize
$ synth -top dff_const1
//There will be a separate flop library under a standard library
//so we need to tell the design where to specifically pick up the DFF
//But here we point back to the same library and tool looks only for DFF instead of all cells
$ dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```
#### (i)dff_const1.v

**_Expected logic from verilog file_**

<img width="400" alt="dff_const1" src="https://user-images.githubusercontent.com/93824690/166229653-c4399f44-b047-4ce5-9231-08d11b0da7f6.png">

**_GTK Wave_**

<img width="400" alt="const1 gtkwave" src="https://user-images.githubusercontent.com/93824690/166229663-62940883-a7d8-4948-8500-846e3adba739.png">

**_Statistics showing a flop inferred_**

<img width="400" alt="Screenshot (217)" src="https://user-images.githubusercontent.com/93824690/166231858-6c8328e6-75d7-4c49-b7de-dfa40a785b6c.png">
																		  
**_Realization of Logic_**																	

<img width="400" alt="Screenshot (216)" src="https://user-images.githubusercontent.com/93824690/166231848-275be7b7-7123-44f9-af20-99045cbaf5bd.png">

>_The optimized graphical realization thus shows the flop inferred. Also, the design code has active high reset and the standard cell library has active low reset - so, there is a presence of inverter for the reset._

#### (ii)dff_const2.v

**_Expected logic from verilog file_**

<img width="400" alt="const2 mo" src="https://user-images.githubusercontent.com/93824690/166232931-87c503db-5670-4fe2-a2cc-d04e3b093323.png">

**_GTK Wave_**

<img width="400" alt="const 2 gtk" src= "https://user-images.githubusercontent.com/93824690/166233155-e111c609-f9ca-4241-88e0-784ca36fcd73.png">

**_Statistics showing a flop inferred_**

<img width="400" alt="Screenshot (218)" src="https://user-images.githubusercontent.com/93824690/166231886-062ff0e3-8ae5-44ed-9b5f-1ede18846654.png">

**_Realization of Logic_**

<img width="400" alt="Screenshot (219)" src="https://user-images.githubusercontent.com/93824690/166231903-de4a3bdd-362c-4b62-8d7f-4c882dac6b3c.png">

#### (iii)dff_const3.v

**_Expected logic from verilog file_**

<img width="400" alt="const3 mo" src="https://user-images.githubusercontent.com/93824690/166234186-4880afe5-907c-478c-9431-7ceabe36f776.png">


**_GTK Wave_**

<img width="400" alt="Screenshot (221)" src="https://user-images.githubusercontent.com/93824690/166231920-517d7982-bef0-4684-995b-36a7431c5192.png">

**_Statistics showing a flop inferred_**

<img width="400" alt="const 3 st" src="https://user-images.githubusercontent.com/93824690/166234729-d271f40f-51a9-4f00-a8c6-45b3056db776.png">
																		  
**_Realization of Logic_**

<img width="400" alt="Screenshot (223)" src="https://user-images.githubusercontent.com/93824690/166231948-45a94b52-e593-4128-8958-ec4157806351.png">


																		 
																		 
#### (iv)dff_const4.v

**_Expected logic from verilog file_**

<img width="400" alt="const 4 mo" src="https://user-images.githubusercontent.com/93824690/166235278-42609662-e9d2-4ff7-bb89-b4791aec301a.png">

**_GTK Wave_**

<img width="400" alt="const 4 gtk" src="https://user-images.githubusercontent.com/93824690/166235397-a216da63-c9f9-4df5-a583-471885306443.png">

**_Statistics showing a flop inferred_**

<img width="400" alt="const4 st" src="https://user-images.githubusercontent.com/93824690/166235523-fa43d8ad-be63-4a55-ba2c-1ea5fee035fb.png">

									
**_Realization of Logic_**

<img width="400" alt="const4 re" src="https://user-images.githubusercontent.com/93824690/166235601-3dd5590c-f1a1-419c-ac3b-afe660630354.png">


#### (v)dff_const5.v

**_Expected logic from verilog file_**

<img width="400" alt="const5 mo" src="https://user-images.githubusercontent.com/93824690/166235678-4c4040e4-19af-48a6-9817-606cb1f3390d.png">


**_GTK Wave_**

<img width="400" alt="const 5 gtk" src="https://user-images.githubusercontent.com/93824690/166235800-81062051-7b67-419f-b40a-67b4b7fc6ea3.png">

**_Statistics showing a flop inferred_**

<img width="400" alt="const5 st" src="https://user-images.githubusercontent.com/93824690/166235889-7fe5effa-676a-44a9-9acd-b829fe01816f.png">

						
**_Realization of Logic_**

<img width="400" alt="const 5 re" src="https://user-images.githubusercontent.com/93824690/166235967-b31ef6b1-dbb0-4cd7-8705-78bb3c4b8d20.png">


#### SEQUENTIAL UNUSED OUTPUT OPTIMIZATION
```
//Steps Followed for each of the unused output optimization problems:
//opening the file
$ gvim counter_opt.v
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog opt_check.v
//Synthesize Design - this controls which module to synthesize
$ synth -top opt_check
//To perform constant propogation optimization
$ opt_clean -purge
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```

#### (i) counter_opt.v

**_Expected logic from verilog file_**

<img width="400" alt="count opt mo" src="https://user-images.githubusercontent.com/93824690/166236838-2e8bf699-50bd-45c8-ba09-0d24c8dc8a70.png">

<img width="400" alt="count opt fig" src="https://user-images.githubusercontent.com/93824690/166236846-da9c5ae1-64cd-4db4-bec4-91551f122cdb.png">

>_If there is a reset, the counter is intialised to 0, else it is incremented - performing like an upcounter. Since it is a 3 bit signal, the counter rolls back after 7. However, the final output q is sensing only the count [0], so the bit is toggling in every clock cycle (000, 001, 010 ...111). The other two outputs are unused and does not create any output dependency. Hence, these unused outpus need not be present in the design._

**_Statistics showing only one flop inferred instead of 3 flops sinces it is a 3 bit counter_**

<img width="400" alt="Screenshot (228)" src="https://user-images.githubusercontent.com/93824690/166236963-44e58dc3-2570-4e65-82f5-129412be9686.png">


**_Realization of Logic_**

<img width="400" alt="Screenshot (229)" src="https://user-images.githubusercontent.com/93824690/166236989-56da54ff-5a25-4e0d-8739-a7f1e1c2a004.png">

>_optimized graphical realization output Q (count0) being fed to NOT gate so as to perform the toggle function. The other outputs which has no dependency on the primary out is optimized off._

#### (ii) counter_opt2.v
```
//Steps Followed:
//Copying the code to a new file
$ cp counter_opt.v counter_opt2.v
$ gvim counter_opt2.v
//Changes made in the verilog code, i for insert mode: 
- assign q = [count2:0] == 3'b100;
```

**_Expected logic from verilog file_**

<img width="400" alt="co2 mo" src="https://user-images.githubusercontent.com/93824690/166238342-74ba4df0-f65c-4ca1-ba19-1c284064d1c8.png">

>_In this case, all three bits of the counter is used and hence 3 flops are expected in the optimized netlist._

**_Statistics showing all three flops inferred_**


<img width="400" alt="co 2 st" src="https://user-images.githubusercontent.com/93824690/166238350-4589b69e-c10b-4b1f-bf56-495866548718.png">

**_Realization of Logic_**

<img width="400" alt="Screenshot (230)" src="https://user-images.githubusercontent.com/93824690/166237000-29183be5-d656-4900-ab10-107cbf57ed21.png">

>_All three flops can be seen. There is a need for incremental logic, so the logic other than flops represent the adder circuit. The expression at the output is 
q = counter2.counter1'.counter0'. Therefore, the outputs having no direct role on the primary output will only be optimized away._

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 4. Day 4 - Gate Level Simulation(GLS), Blocking vs Non-blocking and Synthesis-Simulation Mismatch

### 4.1.GLS, Synthesis-Simulation mismatch and Blocking/Non-blocking statements
**What is Gate Level Simulation (GLS) ?**
Running the testbench against the synthesized netlist ouput as a DUT is known as Gate Level Simulation (GLS). The Output netlist should logically be same as the RTL code so that the testbench will align itself when we simulate both the files to obtain the waveforms.
**_Advantages of GLS:_**
 *To logically verify the correctness of the design after Synthesis.
 *During the RTL Simulation, timing was not accounted. But for practical applications, there is a need to ensure the timing of the design to be met.
 
**Why GLS?**
GLS is required to verify the logical correctness of the design post synthesis with the help of the netlist file. It ensures whether the timing of the design is met and for thi, the GLS used to run with delay annotations.

<img width="400" alt="GLS" src="https://user-images.githubusercontent.com/93824690/166241284-6638d96e-4e21-4c93-a238-ed43834ecf37.png">
```
//consider a netlist
and uand (.a(a),.b(b))
or uor (.a(a),.b(b))
//There is a need to define the meaning of and and or
//Thus we need netlist, testbench and verilog models of the standard cells
```
>_Netlist consists of all standard cells instantiated and it's meaning is conveyed to the iVerilog using Gate Level Verilog Models. Gate Level Verilog Models can be functional or timing aware. If the gate level models are delay annotated, then GLS can be performed for timing validation also in addition to functional validation._

#### SYNTHESIS SIMULATION MISMATCH

If netlist is a true reciprocation of RTL, what is the need to validate the functionality of netlist? There may be synthesis and simulation mismatch due to the following reasons:

**(I)Missing Sensitivity List
 (II)Blocking Vs Non Blocking Assignments
 (III)Non Standard Verilog Coding**

**(I)Missing Sensitivity List**
```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (sel)
begin
   if (sel)
            y = i1;
   else 
            y = i0;          
end
endmodule
```
The output of Simulator changes only when the input changes. The output is not evaluated when there is no activity. In the above 2x1 mux code, when select is changing (when select is 1), the output is 1 when input is 1 else the output is 0. The always block evaluates only when there is a transition change in select pin, and is not sensitive (output does not reflect) to changes in the inputs 0 and 1.

**_Corrected code for missing sensitivity list:_**
```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (*)
begin
   if (sel)
            y = i1;
   else 
            y = i0;        
end
endmodule
```
>_mismatch is corrected by having always @ (*) where the always block is evaluated when any signal changes. So, any changes in inputs will also be seen in the output._

#### (II)Blocking Vs Non Blocking Assignments in Verilog

Blocking and Non-blocking statements are procedural assignment statements that can be implemented only inside an always block.

*Blocking Assignments --> =
   *Executes the statements in the order in which they are coded.
*Non-blocking Assignments --> <=
   *Executes the RHS of all such assignments when the always block is entered and assigned to LHS in a parallel evaluation.
   
Synthesis-Simulation mismatches due to incorrect ordering of the blocking assignments done inside an always block.

#### CAVEAT 1:-
```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q = q0;
        q0 = d;    
end
endmodule
```
<img width="400" alt="cav1" src="https://user-images.githubusercontent.com/93824690/166245183-8f2608db-88cb-4966-beba-5038ff45390b.png">

>_The assignments inside the code represent the blocking statements. q0 and q are assigned to 1 bit 0s - so asynchronous reset connection happens. However, in the later parts, q0 is assigned to q and then d gets assigned to q0. If suppose, there is a change in the code._
```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q0 = d;
        q = q0;    
end
endmodule
```
>_In this case, d is assigned to q0 and then q0 is assigned to q. So, by the time the second statment gets executed, q0 has the value of d. This will lead to implementation of only one flop. Previously, q has the value of q0 and q0 has the value of d - which lead to implementation of 2 storage elements._

#### _Always Use Non- Blocking Statements when writng the Sequential circuits code_

#### CAVEAT 2:- Causing Synthesis Simulation Mismatch
```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        y = q0 & c;
        q0 = a|b ;    
end 
endmodule
```
>_The code is aimed to create a function of y = (A+B).C. In the above code, when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So y gets evaluated first (q0.C), where the q0 results corresponds to the previous iteration's result. The q0 value gets updated only in the second statement._

When the order of the statements is changed: In this case, a OR b is evaluated first and the latest value is used for calculating y.
```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        q0 = a|b ;
        y = q0 & c;
end 
endmodule
```
#### > Therefore there is a paramount importance to run the GLS on the netlist and match the specifications, to ensure there is no simulation synthesis mismatch.####

#### (i) ternary_operator.v
>_Note: Mux function is written using a ternary operator. Ternary operator takes 3 operands with the format.
```
<Condition>?<True>:<False>
```
**_Verilog File_**

<img width="641" alt="Screenshot (288)" src="https://user-images.githubusercontent.com/93824690/166249218-f94c6544-cef7-4861-853b-9e99f0f250f5.png">

**_GTK Wave_**
<img width="641" alt="Screenshot (237)" src="https://user-images.githubusercontent.com/93824690/166249367-e4b5567f-f683-4a5e-89fb-b875ad6eeaf4.png">

**_Statistics_**

<img width="400" alt="ter st" src="https://user-images.githubusercontent.com/93824690/166249983-99797adb-801c-43d4-983d-4c8154f34cbd.png">

**_Realization of Logic_**

<img width="641" alt="Screenshot (238)"src="https://user-images.githubusercontent.com/93824690/166249396-10ea2950-f074-4f53-b8e1-296fea18491d.png">

>_NAND gate with i1 and sel, inverted io and Or to And invert gate, to which the inputs are sel and inverted i0. The output y is given by the expression = sel'.i0 + sel.i1_

**_Commands to perform Gate Level Simulation_**

<img width="400" alt="command gls" src="https://user-images.githubusercontent.com/93824690/166250785-3f96dd41-2f7a-4440-baa8-0a3749853415.png">

**_GLS OUTPUT_**

<img width="641" alt="gls gtk" src="https://user-images.githubusercontent.com/93824690/166250831-c7b446ef-d96e-46a0-893c-edf6e9780d30.png">

#### MISSING SENSITIVITY LIST
#### (ii): bad_mux.v showing mismatch due to missing sensitivity list

**_Verilog File_**

<img width="400" alt="bad mo" src="https://user-images.githubusercontent.com/93824690/166252999-87ca21b7-ef7e-4f61-98fa-3167fa3be26f.png">

>_during Simulation, the logic acts as a latch and during synthesis, it acts as a mux._

**_GTK Wave_**

<img width="400" alt="bad gtk" src="https://user-images.githubusercontent.com/93824690/166253027-c182567f-82ff-483a-af77-aabdf6e1d2cd.png">

**_Synthesis Statistics_**

<img width="400" alt="bad st" src="https://user-images.githubusercontent.com/93824690/166253012-6caad5c4-e707-454b-b2f8-d5f1f58885cd.png">

**_GLS Output_**


<img width="400" alt="bad gls" src="https://user-images.githubusercontent.com/93824690/166253052-a961d8b0-dc45-4b4a-a90e-ff5409fa0448.png">


**_Realization of Logic_**

<img width="400" alt="Screenshot (241)" src="https://user-images.githubusercontent.com/93824690/166253100-20d54e2c-58b6-479c-bb7a-296833f7d4ee.png">

>_Confirms the functionality of 2x1 mux after synthesis where when the select is low, activity of input 0 is reflected on y. Similarly, when the select is hight, activity of input 1 is reflected on y. Hence there is a synthesis simulation mismatch due to missing sensitivity list._

#### CAVEATS IN BLOCKING ASSIGNMENTS
#### (iii) blocking_caveat.v showing mismatch due to blocking assignments

**_Verilog File_**

<img width="400" alt="cav mo" src="https://user-images.githubusercontent.com/93824690/166254558-27b5559d-cf33-4089-9863-116efb61f9f3.png">

>_when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So d gets evaluated first (x.c), where the x results corresponds to the previous iteration's result (a|b). The d value gets updated only in the second statement. The output expression is given as d = (a+b).c_

**_Synthesis Statistics_**

<img width="400" alt="cav st" src="https://user-images.githubusercontent.com/93824690/166254570-e078ddec-8f95-4aba-ac01-4d552eb742a3.png">

**_GTK Wave_**

<img width="400" alt="cav gtk" src="https://user-images.githubusercontent.com/93824690/166254577-420c3b5f-2e2e-43ba-a670-f41a828e0e75.png">

>_d = (a+b).c, if the inputs a,b = 0; then a+b = 0. The output d = 0. But, we observe the output d = 1 because it looks at the past value where a+b was 1._
**_GLS Output_**

<img width="400" alt="cav gls" src="https://user-images.githubusercontent.com/93824690/166254673-5b2ff6aa-4aea-4ca1-bb06-28079dc50910.png">

**_Realization of Logic_**

<img width="400" alt="cav re" src="https://user-images.githubusercontent.com/93824690/166254689-2095c677-6fe6-4264-b840-880b8017675b.png">

<_value of output d is 0 after simulation and 1 after synthesis for the same set of input values. Hence there is a synthesis simulation mismatch due to blocking assignments._

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## 5. Day 5 - Optimization in synthesis

### 5.1 If Case constructs
The if statement is a conditional statement which uses boolean conditions to determine which blocks of verilog code to execute. If always translates into Multiplexer. It is used for priority Logic and are always used inside always block.The variable should be assigned as a register.

**_Syntax for IF Statement_**
```
if<cond>
begin
.....
.....
end
else
begin
.....
.....
end
```
**_Syntax for IF- ELSE-IF Statement_**
```
if<cond1>
begin
.....
 executes cb1
.....
end
else if<cond2>
begin
.....
  executes cb2
.....
end
else if<cond3>
begin
.....
  executes cb3
.....
end
else
begin
.....
  executes cb4
.....
end
```
**_Hardware Implementation_**

<img width="400" alt="hd imp" src="https://user-images.githubusercontent.com/93824690/166259138-8166cfbc-1ae2-4260-87ad-4aca82c56c94.png">

**_Cautions with using IF Statements_**
Inferred latches can serve as a 'warning sign' that the logic design might not be implemented as intended. They represent a bad coding style, which happens because of incomplete if statements/crucial statements missing in the design. For ex: if a else statement is missing in the logic code, the hardware has not been informed on the decision, and hence it will latch and will tried retain the value. This type of design should be completely avoided unless intended to meet the design functionality (ex: Counter Design).

<img width="400" alt="inf if" src="https://user-images.githubusercontent.com/93824690/166259507-9a71c456-5108-42dc-ad1c-1ebb6e9b0d6f.png">

#### CASE Statements
The hardware implementation is a Multiplexer. Similar to IF Statements, Case statements are also used inside always block and the variable should be a register variable.

**Case Statements**
```
_reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
		      .
	endcase	
end
```
**Caveats in CASE Statements**

*Case statements are dangerous when there is an incomplete Case Statement Structure may lead to inferred latches. To avoid inferred latches, code Case with default conditions. 
```
reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
	default:begin
         ....
		       end
	endcase	
end
```
*Partial Assignments in Case statements - not specifying the values. This will also create inferred latches. To avoid inferred latches, assign all the inputs in all the segments of the case statement.

### 5.2 INCOMPLETE IF STATEMENTS

#### (1) incomplete if statements

**_Verilog File_**

<img width="400" alt="100" src="https://user-images.githubusercontent.com/93824690/166263361-bd07fa49-2177-47a7-96d2-5a23e67e9a30.png">

**_GTK Wave_**

<img width="400" alt="Screenshot (250)" src="https://user-images.githubusercontent.com/93824690/166262139-4b40b5d0-55e9-448b-968f-1bc5dbd59c4a.png">

>_Else case is missing so there will be a D latch._
**_Synthesis Statistics_**																		  
<img width="400" alt="Screenshot (251)" src="https://user-images.githubusercontent.com/93824690/166262175-01dc3362-8128-43a2-bda4-14be20b26d7d.png">

**_Realization of Logic_**
<img width="400" alt="Screenshot (252)" src="https://user-images.githubusercontent.com/93824690/166262206-b46f9ea8-6fec-4c6f-a1ce-bae4dafc566a.png">

>_synthesized design has a D Latch inferred due to incomplete if structure (missing else statement)._

#### (Case2) incomplete if statements

**_Verilog File_**

<img width="400" alt="200" src="https://user-images.githubusercontent.com/93824690/166263847-c791f930-eeb1-45ff-8052-f1d247b8bebd.png">

**_GTK Wave_**

<img width="400" alt="Screenshot (254)" src="https://user-images.githubusercontent.com/93824690/166262258-db09ddf3-47d0-44d3-888c-6b45b56d1cdf.png">

>_When i0 is high, the output follows i1. When i0 is low, the output latches to a constant value (when both i0 and i2 are 0). Presence of inferred latches due to incomplete if structure._

**_Synthesis Statistics_**

<img width="400" alt="Screenshot (256)" src="https://user-images.githubusercontent.com/93824690/166262277-2398c16a-7007-4b1e-937d-398989c891f6.png">

**_Realization of Logic_**

<img width="400" alt="Screenshot (257)" src="https://user-images.githubusercontent.com/93824690/166262309-0c66e904-20fb-49ba-859a-97667ca61996.png">


### 5.2 INCOMPLETE CASE STATEMENTS

#### CASE 1: incomplete case statements
**_Verilog File_**

<img width="400" alt="33" src="https://user-images.githubusercontent.com/93824690/166265632-97eb0312-e58f-40e6-9f26-fa6b54abb8ca.png">

**_GTK Wave_**
<img width="400" alt="Screenshot (258)" src="https://user-images.githubusercontent.com/93824690/166264918-dab97bee-102e-43b2-9887-d8d07419b281.png">

>_When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the ouput latches to the previously available value._

**_Synthesis Statistics_**

<img width="400" alt="i st" src="https://user-images.githubusercontent.com/93824690/166266006-0a03e9e8-0fc9-45bf-be6a-b82edbaa6f5c.png">

**_Realization of Logic_**

<img width="400" alt="Screenshot (259)" src="https://user-images.githubusercontent.com/93824690/166264934-397bc9a5-7e18-419c-898b-14d910aeafe5.png">

>_The synthesized design has a D Latch inferred due to incomplete case structure (missing output definition for 2 of the select statements)._

#### CASE 2: incomplete case statements with default

**_Verilog File_**

<img width="400" alt="comp case mo" src="https://user-images.githubusercontent.com/93824690/166266795-cf2d4b1c-76f3-434f-a86a-873ead2f66e8.png">

**_Synthesis Statistics_**

<img width="400" alt="comp st" src="https://user-images.githubusercontent.com/93824690/166266812-2ae22097-2241-49a3-8a10-96bab1d0ff09.png">

**_GTK Wave_**

<img width="400" alt="Screenshot (260)" src="https://user-images.githubusercontent.com/93824690/166264948-65dc25ba-0bdd-4ff2-b6de-8b3616d665f5.png">

>_When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the presence of default sets the output to i2 when the select line is 10 or 11. The ouput will not latch and be a proper combinational circuit._
**_Realization of Logic_**

<img width="400" alt="Screenshot (261)" src="https://user-images.githubusercontent.com/93824690/166264984-ce6f0d0d-473f-409c-9149-f37b5b82234b.png">

#### CASE 2: partial case statement
**_Verilog File_**

<img width="400" alt="p mo" src="https://user-images.githubusercontent.com/93824690/166267883-dfbcec82-f803-458d-bec6-81cd09ad7d4e.png">

**_Synthesis Statistics_**

<img width="400" alt="p st" src="https://user-images.githubusercontent.com/93824690/166267889-5976618e-4223-470e-b9ad-2a33636b8ba6.png">

**_Realization of Logic_**

<img width="400" alt="p re" src="https://user-images.githubusercontent.com/93824690/166267900-ca0787fd-c00b-4a33-96b4-7ea15e5f1c41.png">



















































