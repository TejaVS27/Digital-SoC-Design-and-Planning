# Digital-SoC-Design-and-Planning
This project aims to implement a RISC-V SoC design from RTL to GDSII using open-source EDA tools.
> 1. OpenLANE : Open source automated ASIC Flow tool from synthesis to GDSII
> 2. Sky130 PDK : Open source process design kit from SkyWater Technology based on 130nm CMOS technology.

## Day-1: Getting Hands-On with OpenLANE and Sky130
### Design preparation:

```bash
# Entering the Working Directory: 
$ cd Desktop/work/tools/openlane_working_dir/openlane
```
This directory containes all the supporting files for the OpenLANE flow and also a few set of designs.  
______________________________________

```bash
# Entering the pdk Directory: 
$ cd Desktop/work/tools/openlane_working_dir/pdks
```
This directory containes all the files related to sky130 PDK.  
Here, for our design we will be using sky130_fd_sc_hd standard cell library of sky130 PDK. Also there are other SCL along with this like hs, ms, ls, hdll etc.  
__________________________________

pwd : Desktop/work/tools/openlane_working_dir/openlane  
  
The design we will be working on is __"picorv32a"__ in designs directory.  
This design directory consists of source files (.v and .sdc) along with design config and SCL config .tcl files.  
Now in current working directory we shall invoke the tool through following commands.
```bash
$ docker
$ ./flow.tcl -interactive
% package require openlane 0.9
```
![Screenshot 2024-04-13 125643](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/8607768d-dce5-4088-9de4-3dffae101893)

### Synthesizing the design: 
__TOOL : Yosys, abc__  

Before running the synthesis tool, we shall prepare required files structure in the design,
```bash
% prep -design picorv32a
```
This creates a folder __runs__ in the picorv32a directory, with a folder which constains the required files structure for the flow.

![Screenshot 2024-04-13 130609](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/5967b337-7e18-449a-a332-ea7a3ec878e1)
![Screenshot 2024-04-13 130721](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/2d66b401-1b6a-4bc1-a93f-ad9a3ecfae81)


Now to run synthesis, use command:
```bash
run_synthesis
```

![Screenshot 2024-04-13 131247](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/c076780c-00c6-40ff-8ec2-8d90ea827b8b)

#### Generated files
Gate level netlist file is generated after synthesis, can be found in the results/synthesis folder  
pwd: openlane/designs/picorv32a/runs/13-04_07-35/results/synthesis  
```bash
$ gedit picorv32a.synthesis.v
```
run the above command to view the GLN file in text editor.

Apart from this file, few reports are also generated and can be found in the reports/synthesis directory.  
Area report (with no. of different cells), Timing report (with min path and max path slack), Lint report etc.

![Screenshot 2024-04-13 131940](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/a1369a1f-c1ea-4eda-a7a5-abc1ab803d05)

![Screenshot 2024-04-13 131842](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/3bea9c13-4b21-42d6-a2f4-b564ace5ef31)

Flop ratio = no. of dff / no. of cells = 1613 / 14876 = 10.84%

## Day-2: Floorplanning and placement  
### Floorplan
Use `run_floorplan` command to run floorplan after completing the synthesis.

![Screenshot 2024-04-21 162313](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/daff124a-1d56-4469-be52-3957f0d74773)
![Screenshot 2024-04-21 162922](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/8d224da1-759b-43c0-a281-0ad1cf1053d2)

Note: Synthesis was done again before floorplan to create synthesis files in `21-04_10-36` directory in runs directory.  

To view the floorplan generated files goto following path:  
pwd: openlane/designs/picorv32a/runs/21-04_10-36/results/floorplan  
  
we can see the generated .def file

![Screenshot 2024-04-21 164405](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/ff8255bf-9011-41ba-bbd1-f8fe73d6a877)

To view the Layout after floorplan, we use `Magic` tool through following command in the results/floorplan directory  

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
```

![Screenshot 2024-04-21 170903](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/5829e4b2-2639-4d16-ac38-3ae92ecd443a)

we can select any pin or an instance and give a `% what` command in Tkcon.tcl tab to view its details.

![Screenshot 2024-04-21 171152](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/55db2e2a-5fc2-458d-9677-95d39b45bc0a)

### Placement

Use `run_floorplan` command to run placement after completing the floorplan.

![Screenshot 2024-04-21 220724](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/c682b268-6f7b-4848-ac76-049ef75537c6)

Both Global and Detailed placement was completed, To view placement generated files go to following path  

pwd: openlane/designs/picorv32a/runs/21-04_10-36/results/placement  

we can see the generated .def file  

To view the Layout after floorplan, we use `Magic` tool through following command in the results/placement directory  

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```

![Screenshot 2024-04-21 221842](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/494a6b59-4631-4d92-8b29-d00f2de44ebe)  

![Screenshot 2024-04-21 222125](https://github.com/TejaVS27/RISCV-SoC-Design/assets/124818692/63567bdc-f032-44af-9e13-09c4b5b61964)

## Day-3: Inverter library cell design and characterization:

![Screenshot 2024-04-22 224621](https://github.com/TejaVS27/Digital-SoC-Design-and-Planning/assets/124818692/8a1eef13-cb80-49c4-a19d-f7cdd532b049)

![Screenshot 2024-05-06 233059](https://github.com/TejaVS27/Digital-SoC-Design-and-Planning/assets/124818692/42e385a3-951d-4811-b050-ff8d42d37e44)

![Screenshot 2024-05-06 233133](https://github.com/TejaVS27/Digital-SoC-Design-and-Planning/assets/124818692/30b7858e-1ed2-47fa-a8cd-48605d8138bc)

Rise transition : 2.23963e-09 - 2.1803e-09 = 0.05933e-09  
Fall transistion : 4.09319e-09 - 4.05067e-09 = 0.04252e-09  
Rise delay : 2.212e-09 - 2.152e-09 = 0.06e-09  
Fall delay : 4.05075e-09 - 4.05e-09 = 0.00075e-09  
