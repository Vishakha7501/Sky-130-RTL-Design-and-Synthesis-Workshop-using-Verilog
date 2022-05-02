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






### 3.3.1. Optimizations
By this time we have already noticed that our behavioural description (RTL design) goes through optimizations. As an example from our previously discussed design- multiple_modules.v, the sub_module2 which synthesized to be an OR gate but ended up being somethig diffrent. It has been mapped to a cell- 'sky130_fd_sc_hd__lpflow_inputiso1p_1' by Yosys from the Liberty. These optimizations are performed by the synthesis tool minimizing area, power consumption etc. in perspective.  
![](assets/optimization_muliple_modules_1.png)

