Andes Prebuilt Linux Image
==========================

This is a collection of the prebuilt binaries for direct evaluation.  

### Prerequisites

Make sure the platform has been set up as a target, and you can access it with a TCP connection. I.e. an IP:Port pair. For instructions on setting up a target, please refer to BSP User Manual, Chapter 3.  

Also make sure that you have an appropriate toolchain for Linux development from Andes.  You will need gdb to load these binaries to the target.

### Linux image and DTB folders

#### images

This directory contains four different versions of Linux images for 32-/64-bit targets with or without FPU. Choose the one that suits your target.

#### dtb
This directory contains all the DTB files for AE350 targets of different configurations. The naming of the DTB files is self-explanatory for you to select one for your target. For details about the DTB naming, please refer to Linux Development User Manual, Chapter 4.3.

### Example: Boot a 64-bit 4-core SMP system with FPU

Execute the following command:
```
$ riscv64-linux-gdb -ex 'remote target IP:Port' \ # Connect GDB to the target 
                    -ex 'reset-and-hold' \
                    -ex 'restore dtb/ae350_c4_64_d.dtb binary 0x20000000" \ # Load the DTB into RAM
                    -ex 'reestore images/rv64v5d/ae350_rv64_smp_Image binary 0x200000" \ # Load the Image into RAM
                    -ex 'thread apply all set $pc=0x0' \
                    -ex 'thread apply all set $a0=$mhartid' \
                    -ex 'thread apply all set $a1=0x20000000' \
                    -ex 'load' \
                    -ex 'c' images/rv64v5d/fw_jump.elf
```
