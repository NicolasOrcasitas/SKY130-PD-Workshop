# SKY130-PD-Workshop
Here there are all the material and labs made in the SKY130 PD Workshop.

## Table of Contents

[Day 1](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/README.md#day-1-inception-of-open-source-eda-openlane-and-sky130-pdk)

[Day 2](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/README.md#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)

<a name="Day 1"></a>
# Day 1: Inception of open-source EDA, OpenLANE and SKY130 PDK

## Getting to know OpenLane directory
First we go to the folder where the openlane and pdks folders are located. For this case the adress was:

> cd work/tools/openlane_working_dir/

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Adress_OpenLane_PDK.png?raw=true)

### pdks's folder
 If we get into the pdks folder, we will see three folders that have all the pdks necesary for the openlane flow.
 
 ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/OpenPDKS.png?raw=true)
 
 #### skywater-pdk
 Inside this folder there are all the pdks of skywater, all the timing, .lef, .tech, and information files.
 
 #### open_pdks
 Inside this folder there ares some scripts that make the skywater pdk compatible with the silicon foundries pdks.
 
 #### sky130A
 Inside this folder there are to more folders: libs.tech and libs.ref. The first one contains files for the tools used in the openlane flow, like magic, netgen, kalayout, etc. 
 
  ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_sky130A_libs.tech.png)
 
 And in the second one there are the files with information about the technology.
 
 ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_sky130A_libs.ref.png) 


### openlane folder
Here is where we are going to run all the openlane flow.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_openlane.png)

Inside this folder there is the designs folder, where all the design are located. In this case we are going to work with picorv32a design.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_picorv32a.png)

#### scr
Here there is the verilog file.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_scr.png)


#### config.tcl
This file contains some information about the design like clock period and location of the design files.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_config.png)

#### sky130A_sky130_fd_sc_hd_config.tcl
This file has same information than the config.tcl, however this one is more relevant for the openlane flow.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Open_sky130_hd_tcl.png)

## Starting openlane

For starting openlane we open a new terminal window and run docker. In the docker's bash, we start openlane interactively, and in the openlane's bash we prepare for runing the flow.

>$ docker

>bash-4.2$ ./flow.tcl -interactive

>% package require openlane 0.9

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Starting_openlane_docker.png)


## Preparing for synthesis
Before synthesis it is necesary to get ready. Some folders and files must be created for the synthesis and th other steps of th flow.

> prep -design picorv32a

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/prep_for_synthesis.png)

When the preparation is ready a folder called "runs" will be created in the picorv32a folder. Inside this folder there are all the folders needed for storing all the reports and files generated by all the steps of th openlane flow.  

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/Creation_run_folder.png)

## Runing synthesis
Once we are ready, we run the synthesis. This will take no more than 5 minutes.

> run_synthesis

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/synthesis_done.png)

When synthesis is completed there will appear some reports inside the runs folder that was already mention.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/reports.png?raw=true)

We go to the synthesis folder to see the generated reports.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/reports_synthesis.png)

For this lab we will see the statistics report, in order to know the number of flip-flop and get the flop rate.
 
 $$ FR = \frac{No\ flip\ flop}{No\ cells} $$
 
 ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/reports_synthesis_statistics_1.png)
 ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/reports_synthesis_statistics_2.png)
 ![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day1/reports_synthesis_statistics_3.png)
 
 From this report we can get the FR, and the total chip area of the module.
 
 No of flip-flop = 1613
 
 No of cells = 14876
 
 FR = 0.10843
 
 Chip area = 147712.9184


# Day 2: Good floorplan vs bad floorplan and introduction to library cells

## Floor planing

This step of the openlane flow places all the standard cells, logical gates, and pins on the silicon chip. Also make sure that each module is located in a good way, taking into acount their dimentions giving a minimum space for each module, the netlist to put them as nearest as possible to their conections minimizing the length of the path from the pin to the module or between modules, and always try to use the minimum chip area as posible.

In order to have a good floorplan is recomendable to follow this steps:

1. **Define width and heigh of the core and die**: setting good dimentions for the core and die is important, becasuse more area its more expensive, however if we set small dimentions it won't be space for a good module placing. There are two measurements that should be also specify.
$$Utilization\ factor\ =\ \frac{Area\ ocupied\ by\ netlist}{Total\ area\ of\ the\ core}$$
$$Aspect\ ratio\ =\ \frac{Height}{Width}$$

2. **Define locations of preplaced cells**:  The preplaced cells are cells that are instanced just once, and they can be use multiple times. Some preplaced cells are memory, clock gating, mux and comparator. These cells are placed manually by the user before the automated place of the cells, this automatic placing algorithm will use the remaining space for placing the other cells.

3. **Surround pre-placed cells with decopuling capcitor**:  The decopubling capacitors are between VDD and VSS and they are placed near to the preplaced cells because they are used to stabilize the supply voltage for the preplaced cells when there is a transition from 1 to 0 or 0 to 1. Whithout the decoupling capacitors when there is a transition the VDD voltage decreases because of the amount of require charge.

4. **Power planning**: Here there are made a number of vertical and horizontal VDD and VSS routes for getting closer the suply voltages to each cells.

5. **Pin palcement**: The pins should be placed in a good way in order to have the inputs and outputs pins of somo module near.

## FIles used for floorplan

Inside the work/tools/openlane_working_dir/openlane/configuration folder there is a README file that contains all the variables with an explanation that can be used to edit the floorplan stage depending on what the designer wants to do.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/fp_variables_1.png)
![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/fp_variables_2.png)

In the same folder there is a .tcl file that contains the default values for the floorplan.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/default_fp_param.png)

These parameters can be modified in  work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/04-08_14-15/config.tcl file and the parameter will be overwrited
However the more priority file is the config.tcl located in the project folder.

## Running floorplan 
For running floorplan the next command must be executed in the openlane folder.

> run_floorplan

When the floorplan is done, the results can be checked in the next folder in the .def file.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/runs_results_fp.png)

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/picorv32a_fp_def.png)

In the results we can observe the area of the die in microns.

$$DIAREA (660.685\mu m 671.405\mu m)$$ 

$$Total area = 443587.21 \mu m^2$$

To visualized the floorplan we can open magic:

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/magic_view_fp.png)


And we will see this layout

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/magic_view_fp_layout.png)

## Running placement

This stage consist in three steps:

1. **Binding netlist with physical cells**: Here it binds each module of the netlist with a physical cell that have a height and a width

2. **Placement**: Here the physical binded cells are placed on the core in an optimal way in order to minimize the length of the connections between modules.

3.**Optimize placement**: Here th tool calculates the length of the requiered wires and capacitances associated to it, and for long wires buffers are added as repeaters to mantain the value of the signal.

For run placement the next command must be executed.

> run_placement

To visualize the placement we can open magic.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/magic_view_placement.png)

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/magic_view_placement_layout.png)

We can zoom in to see the placed cells

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day2/magic_view_placement_cells_zoom.png)


## Library cells

The library cells contains all the information needed for each step of the flow like dimentions and timing information. These are needed to check the results of each step, for example  in the floorplan the size of the core is decided depending on the number of cells and the dimentions of each one, for timing analisys it takes the timing inforation from the libraries and check setup and hold violations.

Inside these library cells there many standard cells with different characteristics, like sizes, threshhold voltages and gates. The designing standard cell flow is the next one:

1. **Inputs**: The inputs that are needed are the pdks that conatin the rules and specifications stablish by the foundry.
2. **Design steps** : Spice is used for simulated the circuit and also the layout is designed.
3. **Outputs**: The outputs are the spice netlist with capacitances and resistances and the timing, noise and power libs.

## Time characterization 

For timing characterization some concepts must be defined.

| Timing threshhold definitions | Percentage |
| ------------- | ------------- |
| low rise th  | 20% |
| high rise th  | 80% |
| low fall th   | 20% |
| high fall th  | 80%     |
| in rise th    |     50%     |
| out rise th   |     50%     |
| in fall th    |     50%     |
| out fall th   |     50%     |


The values that are used to characterize a cell are.

$$Delay\ time\ =\ out\ th\ -\ in\ th$$

$$Transition\ time\ =\ high\ th\ -\ low\ th$$


# Day 3: Design library cell using Magic Layout and ngspice characterization

## CMOS inverter standard cell

We are not going to design the inverter layout, so we get from the next git hub repository (https://github.com/nickson-jose/vsdstdcelldesign) by nickson-jose.

To copy this repo to our machine, we go to the openlane folder.

> /home/niorcasitas07/Desktop/work/tools/openlane_working_dir/openlane

For cloning the repository.

> git clone https://github.com/nickson-jose/vsdstdcelldesign

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/clonning_github_repo.png)

Then we copy the sky130A.tech file inside the folder of the repository, in order to have it in the same folder at the moment of opening magic.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/coping_sky130a.png)

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/sky130a_inside_cloned_repo.png)

We have all ready for see the inverter layout in magic. To open magic we execute the next command.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/opening_magic.png)

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/inv_magic_open.png)

We can verify that this layout correspond to an inverter layout with the help of magic. Using the command "what" we can check that the upper transistor is a pmos and the lower one is a nmos. In adition all the conections are correctly done, both drains are conected to the output, both gates to the input, the pmos source is conected to VDD, and the nmos source is conected to VSS.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/ptransistor_magic.png)
![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/ntransistor_magic.png)

When we have the layout completed without any drc violations. We proceed to extrac an spice file from the layout, to simulate this cell with ngspice and characterize it.

## Extracting spcie netlist from layout

For extrac the spice file we run the next commands in the magic console.

> extract all

> ext2spice cthresh 0 rthresh 0

> ext2spice

This will create two files with the extensions .ext and .spice

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/spice_file.png)

Inside the spice document there is the netlist extracted from the layout with parasitics.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/text_in_spice_file.png)

## Simulation using ngspice

For running a simulation its necesary to add the simulation profile, the voltage sources as VDD and vin, and the libraries for the transistors. So we edit the .spice file.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/spice_file_edited.png)

For runing the simulation with ngspice we run the next command.

> ngspice sky130_inv.spice

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/run_ngspice_file.png)

The simulation is completed but to see a graphic with the input and output wave we have to run the next command in the ngspice bash.

> plot y vs time a

And we get the plot.

![](https://github.com/NicolasOrcasitas/SKY130-PD-Workshop/blob/main/Day3/plot_y_vs_time.png)

For getting the timing characterization, we use the graphic and the ngspcice bash to get the needed values.

| Timing values | Times |
| ------------- | ------------- |
| rise time  | 63.71 ps |
| fall time  | 42.56 ps |
| rise delay | 60.83 ps |
| fall delay | 27.58 ps |



