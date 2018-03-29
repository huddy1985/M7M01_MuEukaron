<h1 align="center">
	<img width="300" src="https://raw.githubusercontent.com/EDI-Systems/M7M1_MuEukaron/master/Documents/Demo/logo.png" alt="logo">
</h1>

# Unity OS (RME)

点击 **[这里](README_CN.md)** 查看中文版。

&ensp;&ensp;&ensp;&ensp;**RME** is a general-purpose operating system which focuses on many advanced features. This operating system support many advanced features not found in any other OSes, e.g. _FreeRTOS_, _RT-Thread_ or _VxWorks_. On multiple cores, it is as scalable as the Linux kernel. When using the system, the best way is to pull ready-made ports and even binaries from the repository rather than to port or configure by yourself. The advanced features that are intrinsic to this operating system includes:
- [Capability](https://en.wikipedia.org/wiki/Capability-based_security)-based mutable protection domains;
- Massive [scalability](https://en.wikipedia.org/wiki/Scalability) and [parallelism](https://en.wikipedia.org/wiki/Parallel_computing);
- [Fault-tolerance](https://en.wikipedia.org/wiki/Fault_tolerance) and [attack resilience](https://en.wikipedia.org/wiki/Resilience_(network));
- User-level [hierachical scheduling](https://en.wikipedia.org/wiki/Hierarchical_control_system);
- [Full virtualization](https://en.wikipedia.org/wiki/Full_virtualization) and [container](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)-based [paravirtualization](https://en.wikipedia.org/wiki/Paravirtualization);
- [Non-volatile memory (NVM)](https://en.wikipedia.org/wiki/Non-volatile_memory) based systems;
- Real-time multi-core [mixed-criticality](https://en.wikipedia.org/wiki/Mixed_criticality) systems, and so on.

&ensp;&ensp;&ensp;&ensp;The manual of the operating system can be found **[here](https://github.com/EDI-Systems/M5P1_MuProkaron/blob/master/Documents/M7M1_Microkernel-RTOS-User-Manual.pdf)**.

&ensp;&ensp;&ensp;&ensp;Read **[Contributing](CONTRIBUTING.md)** and **[Code of Conduct](CODE_OF_CONDUCT.md)** if you want to contribute, and **[Pull Request Template](PULL_REQUEST_TEMPLATE.md)** when you make pull requests.
This software is **triple-licensed**: it is either **[LGPL v3](LICENSE.md)** or **[modified MIT license](MODMIT.md)**. **Commercial** licenses are also available upon request.

&ensp;&ensp;&ensp;&ensp;For vendor-supplied packages and hardware abstraction libraries, please refer to the **[M0P0_Library](https://github.com/EDI-Systems/M0P0_Library)** repo to download and use them properly.

## Introduction to capability-based multi-core systems

### What are capabilities?
&ensp;&ensp;&ensp;&ensp;Capabilities were a kind of certificate initially introduced into multi-user computer systems to control access permissions. It is an unforgeable token that points to some resource and carries the power to allow operations to the object. In some sense, the Unix file descriptor can be treated as a type of capability; the Windows access permissions can also be viewed as a type of capability. Generally speaking, capabilities are fat pointers that points to some resources.  We guarantee the safety of the system by the three rules:
- Capabilities cannot be modified at user-level;
- Capabilities can only be transfered between different processes with well-defined interfaces;
- Capabilities will only be given to processes that can operate on the corresponding resources.

### Why do we need capability-based systems?
&ensp;&ensp;&ensp;&ensp;The idea of capability is nothing new. Thousands of years ago, kings and emperors have made dedicated tokens for their generals to command a specific branch or group of their army. Usually, these tokens will contain unforgeable (or at least, very difficult to fake) alphabets or characters indicating what powers the general should have, and which army can they command, thus safely handing the army commanding duty off to the generals. In the same sense, capability-based systems can provide a very fine grain of resource management in a very elegant way. By exporting policy through combinations of different capabilities to the user-level, capability-based systems reach a much greater level of flexibity when compared to traditional Unix systems. Additional benefits include increased isolation, fault confinement and ease of formal analysis.

### Wouldn't the microkernel design harm system execution efficiency?
&ensp;&ensp;&ensp;&ensp;Short answer: **No**.  
&ensp;&ensp;&ensp;&ensp;Long answer: If **designed carefully and used correctly** (especially the communication mechanisms), it would instead **greatly boost performance** in multiple aspects, because the fast-paths are much more aggressively optimized now. For example, on some architectures, the context switch performance and interrupt response performance can be up to **40x** better than RT-Linux. When user-level library overheads are also included, the result is still **25x** better than RT-Linux.

### How is it possible that the system is lock-free?
&ensp;&ensp;&ensp;&ensp;This is made possible by extensively applying lock-free data structures and atomic operations. For more information, please refer to [this article](https://www.cs.tau.ac.il/~shanir/concurrent-data-structures.pdf).

## Available system components
&ensp;&ensp;&ensp;&ensp;All available components are listed below. If a github link is provided, the component is available for now.  
- **[RVM](https://github.com/EDI-Systems/M7M2_MuAmmonite)**, which is a microcontroller-oriented virtual machine monitor capable of running multiple MCU applications or operating systems simutaneously. Scales up to 64 virtual machines on 1MB On-Chip SRAM.
    - **[RVM/Lib](https://github.com/EDI-Systems/M7M2_MuAmmonite)**, the microcontroller-oriented user-level library for RME.
    - **[RVM/RMP](https://github.com/EDI-Systems/M5P1_MuProkaron)**, a port of the simplistic RMP on RVM, with all functionalities retained.
    - **[RVM/FreeRTOS](https://github.com/EDI-Systems/FreeRTOS)**, a port of the widely-used [FreeRTOS](https://www.freertos.org/) to RVM.
    - **[RVM/RT-Thread](https://github.com/EDI-Systems/rt-thread)**, a port of the promising [RT-Thread](https://www.rt-thread.org/) to RVM, with all frameworks retained.
    - **[RVM/uCOSIII](https://github.com/EDI-Systems/uCOSIII)**, a port of the famous [uC/OS III](https://www.micrium.com/) to RVM. You should have a commercial license to use this port.
    - **[RVM/MicroPython](https://github.com/EDI-Systems/micropython)**, a port of the popular [MicroPython](https://micropython.org/) to RVM.
    - **[RVM/Lua](https://github.com/EDI-Systems/lua)**, a port of the easy-to-use [Lua](https://www.lua.org/) language to RVM.
    - **[RVM/Duktape](https://github.com/EDI-Systems/duktape)**, a port of the emerging [JavaScript](https://github.com/svaarala/duktape) language to RVM.
    - **[RVM/Essentials](https://github.com/EDI-Systems/M5P1_MuProkaron)**, a port of [lwip](https://savannah.nongnu.org/projects/lwip/), [fatfs](http://elm-chan.org/fsw/ff/00index_e.html) and [emWin](https://www.segger.com/products/user-interface/emwin/) to RVM, all packed in one [RMP](https://github.com/EDI-Systems/M5P1_MuProkaron) virtual machine. Be sure to obtain license to use these softwares.

- UVM, which is a multi-core processor oriented virtual machine monitor capable of supporting full-virtualization and container-based virtualization with unprecedented performance.
    - UVM/Lib, the microprocessor-oriented user-level library for RME.
    - UVM/FV, the full virtualization platform constructed with UVM, which comes with similar functionalities as Virtual Box.

## List of system calls


### Typical performance figures for all supported architectures
**Single-core microcontrollers**

|Machine      |Toolchain     |Flash|SRAM|Yield|Asnd1|Asnd2|Sinv|Sret|Isnd|
|:-----------:|:------------:|:---:|:--:|:---:|:---:|:---:|:--:|:--:|:--:|
|Cortex-M4    |Keil uVision 5|     |    |     |     |     |    |    |    |
|Cortex-M7    |Keil uVision 5|     |    |     |     |     |    |    |    |
|Cortex-R4    |TI CCS7       |     |    |     |     |     |    |    |    |
|Cortex-R5    |TI CCS7       |     |    |     |     |     |    |    |    |
|MIPS M14k    |XC32-GCC      |     |    |     |     |     |    |    |    |
  
*Cortex-R4 and Cortex-R5 are listed here as single-core architectures because their main selling point is CPU redundancy, thus from the viewpoint of the programmer they behave as if they have only one core. Dual-core mode of these two processors are not supported.  

&ensp;&ensp;&ensp;&ensp;**Flash and SRAM consumption is calculated in kB, while the other figures are calculated in CPU clock cycles. All values listed here are typical (useful system) values, not minimum values, because minimum values on system size seldom make any real sense. HAL library are also included in the size numbers. The absolute minimum value for microcontroller-profile RME is about 32k ROM/16k RAM.**

<!-- |Cortex-M4    |GCC           |     |    |     |     |     |    |    |    | -->
<!-- |Cortex-M7    |GCC           |     |    |     |     |     |    |    |    | -->
<!-- |Cortex-R4    |GCC           |     |    |     |     |     |    |    |    | -->

- Cortex-M4 is evaluated with STM32F405RGT6.
- Cortex-M7 is evaluated with STM32F767IGT6.
- Cortex-R4 is evaluated with TMS570LS0432.
- Cortex-R5 is evaluated with TMS570LC4357.
- MIPS M14k is evaluated with PIC32MZEFM100.

**Multi-core microcontrollers**

|Machine      |Toolchain     |Flash|SRAM|Yield|Asnd1|Asnd2|Sinv|Sret|Isnd|
|:-----------:|:------------:|:---:|:--:|:---:|:---:|:---:|:--:|:--:|:--:|
|Cortex-R7    |TBD           |     |    |     |     |     |    |    |    |
|Cortex-R8    |TBD           |     |    |     |     |     |    |    |    |
|TMS320C66X   |TI CCS7       |     |    |     |     |     |    |    |    |

&ensp;&ensp;&ensp;&ensp;**Flash and SRAM consumption is calculated in kB, while the other figures are calculated in CPU clock cycles. HAL library are also included in the size numbers. The absolute minimum value for MPU-based microprocessor-profile RME is about 64k ROM/32k RAM.**

- Cortex-R7 is evaluated with TBD.
- Cortex-R8 is evaluated with TBD.
- TMS320C66X is evaluated with TMS320C6678.

**Multi-core application processors (aka. Desktop/server processors)**

|Machine      |Toolchain     |.text|.data|Yield|Asnd1|Asnd2|Sinv|Sret|Isnd|
|:-----------:|:------------:|:---:|:---:|:---:|:---:|:---:|:--:|:--:|:--:|
|Cortex-A7  x4|GCC           |     |     |     |     |     |    |    |    |
|Cortex-A53 x4|GCC           |     |     |     |     |     |    |    |    |
|X86-64(I) x18|GCC           |     |     |     |     |     |    |    |    |
|X86-64(NI)x32|GCC           |     |     |     |     |     |    |    |    |
|X86-64(A) x16|GCC           |     |     |     |     |     |    |    |    |

&ensp;&ensp;&ensp;&ensp;**RAM consumption is calculated in MB, while the other figures are calculated in CPU clock cycles. Necessary software packages and drivers are also included in the size numbers. The absolute minimum value for application processor-profile RME is about 4MB RAM.**

- Cortex-A7 is evaluated with BCM2836, the exact chip used on Raspberry Pi 2.
- Cortex-A53 is evaluated with BCM2837, the exact chip used on Raspberry Pi 3.
- X86-64(I) is evaluated with a machine with 1x I9-7980XE processor and 128GB memory.
- X86-64(NI) is evaluated with a machine with 4x Xeon X7560 processor and 4x8GB memory.
- X86-64(A) is evaluated with a machine with Ryzen 1950X processor and 128GB memory. 

&ensp;&ensp;&ensp;&ensp;In the 3 tables above, all compiler options are the highest optimization (usually -O3) and optimized for time. 
- Yield : The time to yield between different threads.  
- Asnd1 : Intra-process asynchronous send.
- Asnd2 : Inter-process asynchronous send. 
- Sinv  : Synchronous invocation entering time. 
- Sret  : Synchronous invocation returning time. 
- Isnd  : Interrupt asynchronous send time.

## Getting Started

&ensp;&ensp;&ensp;&ensp;These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

&ensp;&ensp;&ensp;&ensp;You need to choose a hardware platform listed above to run the tests. This general-purpose OS focuses on high-performance MCU and CPUs and do not concentrate on lower-end MCUs or legacy MPUs. Do not use QEMU simulator to test the projects because they do not behave correctly in many scenarios.  

&ensp;&ensp;&ensp;&ensp;If you do not have a standalone software platform, you can also use VMMs such as VMware and Virtual Box to try out the x86-64 ISO image.

&ensp;&ensp;&ensp;&ensp;Other platform supports should be simple to implement, however they are not scheduled yet. For Cortex-M or 16-bit microcontrollers, go [M5P1_MuProkaron](https://github.com/EDI-Systems/M5P1_MuProkaron) _Real-Time Kernel_ instead; M5P1 supports all Cortex-Ms and some Cortex-Rs, though without memory protection support.

### Compilation
**For MCUs**  
&ensp;&ensp;&ensp;&ensp;The **Vendor Toolchain** or **GNU Makefile** projects for various microcontrollers are available in the **_Project_** folder. Refer to the readme files in each folder for specific instructions about how to run them. However, keep in mind that some examples may need vendor-specific libraries such as the STM HAL. Some additional drivers may be required too.

**For application processors**  
&ensp;&ensp;&ensp;&ensp;Only GNU makefile projects will be provided, and only GCC is supported at the moment. Other compilers may also be supported as long as it conforms to the GCC conventions.


### Running the tests
**For MCUs**  
&ensp;&ensp;&ensp;&ensp;To run the sample programs, simply download them into the development board and start step-by-step debugging. All hardware the example will use is the serial port, and it is configured for you in the example.

**For application processors**  
&ensp;&ensp;&ensp;&ensp;The boot sequence is different for different processors. For x86-64 architecture, GRUB is used as the bootloader, and you can boot the system with precompiled LiveCD.iso, just like how you would install any operating system (Ubuntu Linux or Windows). For other architectures, 

### Deployment
**For MCUs**  
&ensp;&ensp;&ensp;&ensp;When deploying this into a production system, it is recommended that you read the manual in the **_Documents_** folder carefully to configure all options correctly. It is not recommended to configure the kernel yourself, anyway; it included too many details. Please use the default configuration file as much as possible. Also, read the user guide for the specific platform you are using.

**For application processors**  
&ensp;&ensp;&ensp;&ensp;Deploy it as if you are deploying any other operating system, or bare-metal hypervisor.

## Built With

- Keil uVision 5 (armcc)
- Code composer studio
- GCC/Clang-LLVM

&ensp;&ensp;&ensp;&ensp;Other toolchains are neither recommended nor supported at this point, though it might be possible to support them later on.

## Contributing

&ensp;&ensp;&ensp;&ensp;Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## EDI Project Information
&ensp;&ensp;&ensp;&ensp;Mutate - Mesazoa - Eukaron (M7M1 R3T1)
