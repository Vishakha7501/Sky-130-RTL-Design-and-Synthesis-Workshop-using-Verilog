# RTL Design and Synthesis in Verilog using Sky130 Technology <!-- omit in toc -->
![](VSD_Workshop_Detail.png)


Table of Contents
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

# 1. Day 1 - Introduction to Verilog RTL design and Synthesis
## 1.1. Introduction to open source simulator iverilog
In the digital circuit design, **register-transfer level (RTL)** is a design abstraction which models a synchronous digital circuit in terms of the data flow between hardware register, and the logical operations performed on those signals. RTL abstraction is used in HDL to create high-level representations of a circuit, from which lower-level representations and ultimately actual wiring can be derived.  

**Simulator**: It is a tool which is used for checking the design. In this workshop we are using **iverilog** tool.**Simulation** is the process of creating models that mimic the behavior of the device you are designing (simulation models) and creating models to exercise the device (test benches).
**RTL Design**: It consists of an actual verilog code / a set of verilog codes that have the functionality to meet the required design specifications of the circuit.

**Test Bench**: It is the setup to apply stimulus(test vectors) to design to checks its functionality.

### HOW SIMULATOR WORKS 
**Simulator** looks for changes on input signals and based on that output is evaluated.
![test bench](https://user-images.githubusercontent.com/93824690/166142807-b8b5a040-009f-4610-9691-cc3e5c53bbbe.png)

**Design** may have 1 or more primary inputs and primary outputs but **TB** doesn't have.)

 ### SIMULATION FLOW
![iverilog based](https://user-images.githubusercontent.com/93824690/166142840-4d8a8377-526c-444e-9498-dc76068046fc.png)

**Simulator** continuously checks for changes in the input. If there is an input change, the output is evaluated; else the simulator will never evaluate the output.

## 1.2. Labs using iverilog & gtkwave
 
 ### ENVIRONMENT SETUP

```
#Steps Followed:
//create a directory
$ mkdir VLSI 
//Git Clone vsdflow. 
$ git clone https://github.com/kunalg123/vsdflow.git
//Git Clone sky130RTLDesignAndSynthesisWorkshop. 
$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
![Screenshot (130)](https://user-images.githubusercontent.com/93824690/166142874-66d2c8f4-0fc5-4a82-91ae-b3821f08f56e.png) 
 
**sky130RTLDesignAndSynthesisWorkshop** Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes.

<--We are given a default set of files and libraries shown below to work on using the practical lab instance.

![Screenshot (134)](https://user-images.githubusercontent.com/93824690/166143655-b00175e1-864b-4aac-8c3c-a4f72e388351.png)

### Simulation using iverilog simulator - 2:1 multiplexer rtl design

#### Verilog file of a simple 2:1 multiplexer

![Screenshot (135)](https://user-images.githubusercontent.com/93824690/166143401-cb52b623-5095-45f4-b880-59b88e4c4bca.png)


#### GTKWAVE Analysis

![Screenshot (142)](https://user-images.githubusercontent.com/93824690/166143969-6e8fb91c-fc09-4a8b-b779-5b45b834888c.png)


#### Access Module Files
```
$ gvim tb_good_mux.v -o good_mux.v 
```

![Screen Shot 2021-09-02 at 12 23 28 AM](https://user-images.githubusercontent.com/89927660/131786618-c6d4663f-5375-48dc-aa16-46b70e6797da.png)

## 1.3. Introduction to Yosys & Logic Synthesis

**Synthesizer** is a tool for converting the **RTL** to Netlist and here we are using the **Yosys** Synthesizer.
#### Yosys SETUP
![Yosys](https://user-images.githubusercontent.com/93824690/166144581-f9888922-5b97-467b-bac8-42138d4c8a7e.png)

#### VERIFY THE SYNTHESIS

<img width="641" alt="verify the synthesis" src="https://user-images.githubusercontent.com/93824690/166144585-f308505e-2f1a-468f-aff4-673800445259.png">

### Logic Synthesis

RTL Design - behavioral representation in HDL form for the required specification.

 **Synthesis** - RTL to Gate level translation.
 The design is converted int gates and connections are made. This given outas a file called **netlist**.

>_.lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._

### Faster cells and Slower Cells

A cell delay in the digital logic circuit depends on the load of the circuit which here is Capacitance.

Faster the charging / discharging of the capacitance --> Lesser is the Cell Delay

Inorder to charge/discharge the capacitance faster, we use wider transistors that can source more current. This will help us reduce the cell delay but at the same time, wider transistors consumer more power and area. Similarly, using narrower transistors help in reduced area and power but the circuit will have a higher cell delay. Hence, we have to compromise on area and power if we are to design a circuit with low cell delay.

### Constraints

A Constraint is a guidance file given to a synthesizer inorder to enable an optimum implementation of the logic circuit by selecting the appropriate flavour of cells (fast or slow).

## 1.4. Labs using Yosys and Sky130 PDKs

### Steps for Design Synthesis

![Screen Shot 2021-09-02 at 12 16 52 AM](https://user-images.githubusercontent.com/89927660/131785975-bbc0c874-8b81-4f29-b892-3b279c7dbc6c.png)

### Inference from Synthesis and Execution of netlist generation

![Screenshot (151)](https://user-images.githubusercontent.com/93824690/166145643-8430fe11-9020-44dc-b649-194343b4f955.png)

### Synthesis by using show command

![Screenshot (153)](https://user-images.githubusercontent.com/93824690/166145665-dfcf9e19-d920-4819-bdcd-793b6209da5c.png)

### Writing Netlist

![Screenshot (157)](https://user-images.githubusercontent.com/93824690/166146410-42e93e44-0fba-4540-a819-95afff4e1b8b.png)

# 2. Day 2 - Timing Libs, Hierarchial Vs Flat Synthesis and Efficient Flop Coding Styles
## 2.1. Introduction to timing labs
```
__Command to open the libary file
$ gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
__To shut off the background colors/ syntax off:
: syn off
__To enable the line numbers
: se nu
```
### Library file

![Screenshot (150)](https://user-images.githubusercontent.com/93824690/166147361-8f538de7-24d2-410e-81c2-832710298c5a.png)

### Contents

For a design to work, there are three important parameters that determines how the Silicon works: Process (Variations due to Fabrications), Voltage (Changes in the behavior of the circuit) and Temperature (Sensitivity of semiconductors). Libraries are characterized to model these variations. 

![Screenshot (162)](https://user-images.githubusercontent.com/93824690/166147849-14fa3eaf-0ca9-440c-bbe8-7b52f6cd682e.png)

### Various Flavours of AND Cell

![Screenshot (163)](https://user-images.githubusercontent.com/93824690/166147855-692033f0-08e1-465f-98b6-5a9fba6a3eb0.png)


## 2.2. Hierarchial synthesis vs Flat synthesis 

### Hierarchial synthesis  
```
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
```
**Multiple Modules:** - 2 SubModules
**Staistics of Multiple Modules**

![Screen Shot 2021-09-02 at 5 04 14 PM](https://user-images.githubusercontent.com/89927660/131922463-01b29515-c90d-4431-aa86-f10b15fa8d82.png)

**Realization of the Logic**

![Screen Shot 2021-09-02 at 5 12 46 PM](https://user-images.githubusercontent.com/89927660/131923051-5d882430-fa4a-4b0d-8b70-0857c58f9b34.png)

**Netlist file**

![Screen Shot 2021-09-04 at 4 32 57 AM](https://user-images.githubusercontent.com/89927660/132089951-73857fd9-aba8-40ac-b477-b316c8df5dc4.png)

### Flat synthesis  

```
_To flatten the netlist
$ flatten
_Writing the netlist in a crisp manner and to view it
$ write_verilog -noattr multiple_modules_flat.v
$ !gvim multiple_modules_flat.v
```
**Realization of the Logic**

![Screen Shot 2021-09-02 at 6 14 16 PM](https://user-images.githubusercontent.com/89927660/131927662-d25c4d37-c0c1-41a7-ab14-9399840eb3ee.png)
  
 
**Netlist file**

![Screen Shot 2021-09-02 at 6 10 30 PM](https://user-images.githubusercontent.com/89927660/131927406-1410346f-77bd-4fe0-a589-ffe2bf2f1a53.png)


### SUB MODULE LEVEL SYNTHESIS

Sub-module level synthesis is preferred when there are multiple instances of same module. Sythesizing the same module over several times may not be advantageous with respect to time. Instead, synthsis can be performed for one module, its netlist can be replicated and then stitched together in the top module. This is also used particulary in massive designs using divide and conquer method. 

**Statistics of Sub-module**

![Screen Shot 2021-09-02 at 9 11 28 PM](https://user-images.githubusercontent.com/89927660/131940332-d8272cc3-affb-471d-9dd0-1ad951d86c22.png)

**Graphical Realization of the Logic**

![Screen Shot 2021-09-02 at 9 12 18 PM](https://user-images.githubusercontent.com/89927660/131940277-9fc3e2fe-b185-4d91-9b2d-86e54c1d1768.png)

**NetList File of Sub-module**

![Screen Shot 2021-09-02 at 9 13 33 PM](https://user-images.githubusercontent.com/89927660/131940384-c0bf6a0a-a73c-4c99-95a4-84a7a654e774.png)


### FLIP FLOP OVERVIEW

In a digital design, when an input signal changes state, the output changes after a propogation delay. All logic gates add some delay to singals. These delays cause expected and unwanted transitions in the output, called as **Glitches** where the output value is momentarily different from the expected value. An increased delay in one path can cause glitch when those signals are combined at the output gate. 
 
### 3.1.2. Introduction to standard cell library
On the gate-level the target architecture is usually described by a “Liberty file”. The Liberty file format is an industry standard format that can be used to describe the behaviour and other properties of standard library cells. [source from The Liberty Library Modeling Standard: http://www.opensourceliberty.org/.]  
As a part of SkyWater Open Source PDK, [multiple](https://skywater-pdk.readthedocs.io/en/main/contents/libraries/foundry-provided.html) standard digital cell libraries are provided that cover a range of different target architectures [source: https://github.com/google/skywater-pdk].  
In this workshop we will be using sky130_fd_sc_hd (high density) standard cell library (target architecture) to map our synthesized design from Yosys.  
Some trivial details of our Liberty file are shown below. Although a thorough look into it has to be given to understand its potenetial.  
![Target Archtecture](assets/liberty.png)  
Also let us have a look at some of the variants of 2-input AND cells from the Liberty library.  
Different variants of and2 with specific cell descriptions can be seen.  
![](assets/lib_and2_variants.png)  
[sky130_fd_sc_hd__and2](https://antmicro-skywater-pdk-docs.readthedocs.io/en/test-submodules-in-rtd/contents/libraries/sky130_fd_sc_hd/cells/and2/README.html)  


## 3.2. Hierarchial synthesis vs Flat synthesis
### 3.2.1. Hierarchial synthesis
In hierarchial synthesis the hierarchy of the RTL design is preserved through the final netlist. Let us look at the below RTL design where we have two sub modules, both instantiated by a top modules.  
![](assets/rtl_multiple_modules.png)

Generic synthesis preserves the hierarchy. It can also be seen that the cells are not mapped to sky130 specific technology yet.  
![](assets/synth_multipl_modules_hier.png)

abc tool maps the technology to Sky130 and generates the netlist. It can be seen that the hierarchy is still preserved.  
![](assets/synth_abc_sky130_hier.png)

It is quite interesting to see how tool itself goes through optimization. RTL mutiple modules.v when synthesized with two different versions of yosys- 07 and 0.9 versions.  
The optimization at the technology mapping of sub_modules2.  
Also quite intesting how yosys decided on two different flavors of and2 for sub_module1 in these versions.    
![](assets/yosys_versions.png)

Details of the realized standard sky130 cells in this design can be found from Skywater PDK documentation.  
[2-input AND](https://antmicro-skywater-pdk-docs.readthedocs.io/en/test-submodules-in-rtd/contents/libraries/sky130_fd_sc_hd/cells/and2/README.html)  
[Input isolation, noninverted sleep](https://antmicro-skywater-pdk-docs.readthedocs.io/en/test-submodules-in-rtd/contents/libraries/sky130_fd_sc_hd/cells/lpflow_inputiso1p/README.html)  

### 3.2.2. Selective sub-module level synthesis
Just like synthesizing an RTL design at the top mudule level, it can be synthesized at the sub-module level as well. This level of freedom brings the below advantsges.  
* &emsp;  Multiple instantions: In case of having multiple instantiations in our design, this feature helps to synthesize just one instance and use it iteratively throughout the top level.    
* &emsp;  Massive desgins: In case of massive design, sub-module level synthesis reduces the burden on sythesis tool in-terms of performance and time.  
 
We can take a look at the same example of multiple_modules.v.  Let us synthesize only sub_module1 and see how the final netlist appears.  
We can see that the synth command just looks at the specified sub_module1 alone and sub_module2 is ommitted from synthesis.  
![](assets/synth_submodule1.png)  

The generated synthesis output this time gives only the AND gate for the sub_module1 rtl.  
![](assets/synth_submodule1_out.png)  

The graphical view clearly indicates the netist for synthesized sub_module1.  
![](assets/show_submodule1.png)  


### 3.2.3. Flat synthesis
yosys command flatten :"This pass flattens the design by replacing cells by their implementation. This pass is very similar to the 'techmap' pass. The only difference is that this pass is using the current design as mapping library.  
Cells and/or modules with the 'keep_hierarchy' attribute set will not be flattened by this command." [source: http://yosyshq.net/yosys/cmd_flatten.html]  

As the name suggestes this pass flattens out the design and hence the hierarchy is lost.  
Let us observe the impact of 'flatten' on systhesized output of same RTL design- multiple_modules.v.  
![flatten](asstes/../assets/yosys_flatten.png)  

We can see that the submodules were deleted and hierarchy is no longer preserved.  
Please pay attantion to the interesting netlist generated.  
![](assets/yosys_flatten_multiple_modules.png)  

Graphical view of flattened netlist is hown below.  
![](assets/flat_multiple_modules_show.png)  

## 3.3. Various Flop coding styles and optimization
abc tool maps only the combinational logic cells from the liberty. It doesn't look for the register cells.  
If the RTL design has sequential logic, dfflibmap pass has to be executed before abc pass. dfflibmap pass looks for the register cells in the Liberty and maps to the sequential logic from the synthesis. And then abc pass has to used to complete the mepping for combinatorial logic.  
![](assets/dfflibmap.png)  
![](assets/dfflibmap_abc_liberty.png)  
Without dfflibmap pass if abc pass is executed, and if the design has only registers, abc has nothing to map and warns like below.  
![](assets/no_dfflibmap_abc.png)  
First the register cells must be mapped to the registers that are available on the target architectures. The 
target architecture might not provide all variations of d-type flip-flops with positive and negative clock
edge, high-active and low-active asynchronous set and/or reset, etc. Therefore the process of mapping the
registers might add additional inverters to the design and thus it is important to map the register cells
first. [source: [Yosys manual](https://raw.githubusercontent.com/wiki/jospicant/IceStudio/yosys_manual.pdf)]  

Let us try to understand this with an example RTL design- 'dff_asyncres.v'.  
Let us run synth -top on the design and take a look how it looks. We can see that it just represet a d-flip flop.  
![](assets/dff_asyncres_snyth.png)  
Let us run dfflibmap -liberty pass on this design and see how it looks. dfflibmap mapped a variant of d-flip flop to our design and this led to an additional inverter in the design.  
![](assets/dff_asyncres_dfflibmap_alone.png)  
Now abc -liberty pass has to be run to complete the mapping of combination logic (the inverter cell). This comepletes the mapping.  
![](assets/dfflibmap_abc_both.png)  

### 3.3.1. Optimizations
By this time we have already noticed that our behavioural description (RTL design) goes through optimizations. As an example from our previously discussed design- multiple_modules.v, the sub_module2 which synthesized to be an OR gate but ended up being somethig diffrent. It has been mapped to a cell- 'sky130_fd_sc_hd__lpflow_inputiso1p_1' by Yosys from the Liberty. These optimizations are performed by the synthesis tool minimizing area, power consumption etc. in perspective.  
![](assets/optimization_muliple_modules_1.png)  
