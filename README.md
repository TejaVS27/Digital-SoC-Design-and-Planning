# RISC V-SoC-Design
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


