---
layout: post
title: Notes on embeded platform
---

The key to understanding how the software interacts with the underlying platform devices is the system memory map and the associated register maps of the devices. Some devices are directly visible to software and mapped to physical addresses in the processor's address space, and other devices are attached over a bus, which introduces a level of indirection when we wish to access the device registers.

The address map within the memory address space on Inter system (and in fact in most systems) is split into two separate sub ranges. The first is the address rang that when decoded accesses the DRAM. and the second is a range of addresses that are decoded to select I/O devices. The two address ranges are known as the Main Memory Address Range and the Memory Mapped I/O (MMIO) Ranges. The Memory Mapped I/O Ranges in which I/O devices can reside is further divided into subregions:
*  Fixed Address Memory Mapped Address.
*  PCIe BUS.

The key capabilities required to support the execution of a multitasking operating system on the processor are as follows:

*  A memory subsystem for initial instruction storage and random access memory.
*  An interrupt controller to gather, prioritize, and control generation of interrupts to the processor.
*  A timer; multitasking operating systems (noncooperative) typically rely on at least one timer interrupt to trigger the operating system scheduler.
*  Access to I/O devices, such as graphics controllers, network interfaces, and mouse, keypads.

---

## Why need interrupt?

The processor requires the ability to interact with its environment through a range of input and output devices. The devices usually require a prompt response from the processor in order to service a real world event. The devices need a mechanism to indicate their need for attention to the processor. Interrupts provide this mechanism and avoid the need for the processor to constantly poll the devices to see if they need attention. Given that we often have multiple sources of interrupt on any given platform, an interrupt controller is needed.

---

## Memory

DRAM is a form of volatile storage. The bit value at a memory location is stored in a very small capacitor on the device. A capacitor is much smaller than a logic gate, and therefore the densities are greater than memory technologies using logic such as caches for the processor or SRAM block on an SOC. The DRAM read time is usually far slower than memory that made from logic gates, such as the CPU cache or SRAM devices (both internal or external).

SRAM memory is commonly allocated for a special data structure that very frequently accessed by the processor, or perphaps a temporal streaming data element from an I/O device. In general, these SRAM bloks are not cache-coherent with the main memory system; care must be taken using these areas and they should be mapped to a noncached address space. 
The cells in a CPU cache are often made from high-speed SRAM cells.

## Nonvolatile storage

There are two primary nonvolatile storage technologies in use today:

*  The frist and most prevalent for embedded system is solid state memory
*  The second is magnetic storage media in the form of hard drives

Modern solid state memory is usually called flash memory. It can be erased and reprogrammed by software driver. there are two distinct flash memory device types: NOR flash and NAND flash. 
They are named after the characteristic logic gate used in the construction of the memory cells. One key difference is that NAND flash is much higher density than NOR devices. NOR devices are more reliable and less susceptible to bit loss. With random access read/write capabilities, the NOR flash more naturally supports a mode of operation called eXecute in Place (XIP). This is where a program can run directly from the flash (without first copying the data from flash to DDR memory).
In contrast to NOR flash, NAND flash does not provide a direct random access address interface. The devices work in a page/block mode.
The NOR flash devices can provide either a serial or parallel interface. NAND flash, the programming of a block must occur in serial fashion (incremental ddress programmed in order). The NAND interface is complex, so it may need a controller. 




