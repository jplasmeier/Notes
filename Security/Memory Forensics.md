# Memory Forensics

A collection of notes accompanying "The Art of Memory Forensics" by someoneorother

### Toolkit

* Volatility Framework
* IDA Pro and Hex-Rays (disassembly/decompilation)
* Wireshark
* YARA

Windows:

* Sysinternals Suite
* WinDbg debugger

# Part 1: An Introduction to Memory Forensics

## Chapter 1: Systems Overview

### PC Architecture

#### Physical Organization

The CPU comprises one or more processor cores, a memory management unit, a TLB, and caches. The MMU is responsible for translating CPU memory addresses to main memory addresses. The TLB (translation lookaside buffer caches address translations. 

The memory controller managers communication with of potentially concurrent requests to the main memory. DMA (Direct Memory Access) allows I/O devices to bypass the CPU when interacting with main memory. 

The main memory of a system is implemented with RAM, which is almost always dynamic (DRAM) and volatile (preserving information requires constant electric charge).

#### CPU Architectures

CPU uses linear/virtual address space to interact with memory. Virtual memory is an abstraction of physical address space, and translation is done via page tables. 

The CPU has several general purpose 32-bit registers per core as well as additional registers for specific functions. The EIP register contains the linear address of the next instruction to be executed by the CPU. 

* CR0 - flags that control operation modes of a CPU (i.e. paging control)
* CR1 - reserved (for what?)
* CR2 - the linear address that caused the last page fault 
* CR3 - the physical address of the structure used for address translation
* CR4 - used for architecture extensions (i.e. PAE - Physical Address Extension)

Segmentation is a memory management technique that divides the 32-bit linear address space into many variable-length segments. IA-32 memory references are addressed by a 16-bit segment selector and a 32-bit offset. Segments descriptors are stored in two special CPU registers, the Global Descriptor Table Register (GDTR) and the Local Descriptor Table Register (LDTR). 
