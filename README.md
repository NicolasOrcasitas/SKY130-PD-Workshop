# SKY130-PD-Workshop
Here there are all the material and labs made in the SKY130 PD Workshop.

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

For this lab we will see the statistics report, in order to know the number of flip-flop and get the flip-flpo rate.
 
 $$ FR = \frac{No\ flip\ flop}{No\ cells} $$
