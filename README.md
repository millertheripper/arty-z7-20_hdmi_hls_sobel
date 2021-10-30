# Arty-Z7-20 - HDMI passthrough with HLS AXI stream sobel filter 

## Base HDMI Project derived from 
https://github.com/Digilent/Arty-Z7-20-hdmi-in/tree/v2020.1

This project demonstrates how to use HLS (High Level Synthesis) language to create image filters (Sobel-Filter, Edge Detection). The HLS logic is exported as 
IP core and inserted into the video block design AXI stream. It uses the HDMI input (Tested with camera, Apeman 1080P) and shows the processed result on the HDMI output in live.

## Xilinx Tools Version: 2020.2

## Requirements
------------
* **Arty Z7-20**
* **Vivado and Vitis Installations**
* **Vitis Video Library** 
* **Open CV 3.3.0 installation**
* **Serial Terminal Emulator Application**
* **MicroUSB Cable**
* **2 HDMI Cables**
* **HDMI Capable Monitor/TV**
* **HDMI Capable Source (Apeman 1080p or HDMI output from Laptop)**

## Compile project:
------
Compile everything: 
```bash
make
```

Create and compile Vivado project: 
```bash
make fpga
```

Create and compile Vitis bare metal project, create boot binary
```bash
make vitis
```

Applying changes:
------
Vivado Block Design: From tcl shell:
```bash
write_bd_tcl [get_property DIRECTORY [current_project]]/../source/scripts/bd.tcl -include_layout -force
```

Software Design (Vitis): Put vitis source code under:
* Put all files in ./vitis/src 

BSP directory structure: 
------
```bash
├── build                       <--- Build Outputs (FPGA and Boot binaries)
├── hw                          <--- All source files related to Vivado Design 
│   ├── build                   <--- Vivado Project  
│   ├── scripts                 <--- TCL scripts to recreate project in batch mode
│   │   └── settings.tcl        <--- Set FPGA type, project name, and number of processors for compilation 
│   └── source
│       ├── constraints         <--- Constraints location. Files will be imported during creation
│       ├── hdl                 <--- Put HDL IP blocks (non block design) here
│       ├── ip                  <--- Put IP blocks (used by ip integrator) here
│       └── scripts
│           └── bd.tcl          <--- TCL script to recreate the block design.
└── sw
    ├── petalinux               <--- Petalinux project 
    ├── vitis                   <--- Project folder containing bare metal application 
    │   ├── build               <--- Boot image is located here (BOOT.bin)
    │   ├── scripts             <--- TCL scripts for batch mode
    │   ├── src                 <--- Bare metal source code files
    │   └── ws                  <--- Vitis workspace
    └── xsa                     <--- Hardware description file, exported by vivado
```