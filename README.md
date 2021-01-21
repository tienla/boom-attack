#Spectre attack experiment on RISCV

This repository uses the reference from https://github.com/riscv-boom/boom-attacks

This have been test with BOOM in ZC706 (Baremetal) && VC706(Run on Linux)

Create elf file for Baremetal: Use Makefile.baremetal

Create elf file for Linux with Newlib toolchain: Use Makefile.Newlib

Create elf file for Linux with Glibc toolchain: Use Makefile.GLibc

File content:

inc: header library

link: linker

src:

- spectrev1: Bound Check Bypass
- spectrev2: Branch Target Injection
- spectre-fence: Mitigation using fence
- spectre-SLH: Mitigation using index masking

#How to use

1. On ZC706

Clone the repository

$ git clone https://github.com/riscv-boom/fpga-zynq
$ git checkout bdd16b4642716aa48346bfd30f73a80278eb5849

Perform the following modifications

- Change the vivado version in fpga-zynq/zc706/src/tcl/zc706\_bd.tcl to 2016.4
- Change the boom git url in rocket-chip module (from "https://github.com/ucb-bar/riscv-boom.git" to "https://github.com/riscv-boom/riscv-boom.git")
- If you use the master branch you maybe need to remove BuildCore in line 8 of "fpga-zynq/common/src/main/scala/Configs.scala"
- Run (common command)

$ make init-submodules #update submodule in the repository
$ make rocket #generating Verilog for two-wide boom
$ make project #generate a Vivado project
$ make fpga-images-zybo/boot.bin #Repacking boot.bin

On the terminal

$ ./fesvr-zynq pk hello #Run riscv app
$ ./fesvr-zynq bbl #Boot linux on BOOM Core

Connect the SD card

$ mkdir sd
$ mount /dev/mmblkb0p1 sd/

Boot up the board

- Copy the contents of fpga-zynq/zc706/fpga-images-zc706 to sdcard 
- Insert the sd card and run the board with SW11 SD Boot mode Boot mode (DOWN-DOWN-UP-UP-DOWN)
- Login with account (id:root, pass:root)

Run this command to rebuild the fesvr-zynq (need to clone the riscv-tools in rocket-chip first)

$ make fesvr-zynq

BOOM attack

Please follow these instructions:

- Execute the following commands:

$ git clone https://github.com/tienla/boom-attack
$ cd boom-attack
$ make

- Copy the "baremetal" binaries at "bin/spectre.riscv" into SD and run:

$ ./fesvr-zynq pk spectre.riscv

- If the program could not run, re-check the toolchain.

1. On VC707

Test environment: 

- tee-hardware: https://github.com/uec-hanken/tee-hardware (version: 2109e57ee411b2b4179cbf62fc84549d9b519d76)

Make using Makefile.GLibc or Makefile.Newlib

Copy the elf files into SD Card and load to run on Linux on FPGA

$./spectrev1.riscv

