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
 Inside this folder there are to more folders: libs.tech and libs.ref. The first one contains files for the tools used in the openlane flow, like magic, netgen, kalayout, etc. And in the second one there are the files with information about the technology.
