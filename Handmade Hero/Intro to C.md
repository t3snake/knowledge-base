---
tags:
  - c_programming
  - handmade
---

[Handmade Hero - Intro to C](https://guide.handmadehero.org/intro-to-c/day1/)  

Compiling checks for errors and produces intermediate files which are mostly code that computer can run but still missing then linking is done.  
Linking is process to all files that were compiled like resolving references and packages to exe.  

Windows is a process running on your computer and your process needs to be linked. Linking process starts from WinMain entry point.

Ascii has limited values until 90s or 100s and cant encode more for other languages like 1000s of kanjis for eg, so Unicode encoding was introduced UTF-8, PCs still use ascii?  

Macros do string replace on your code before compiling. (Meta programming).  
Default value on initialization is random value, maybe affected by dev environment.  

Can check disassembly to see assembly conversion of code written.  

char -> 8 bits  \[-128, 127] = 256 values      -> (smallest unit in c)  
unsigned char -> 8bits \[0, 255] = 256 values  
int -> 16 bits \[-32768, 32767] = 65536 values (and so on)  

Registers only need to be used if CPU needs to do math, memory transfers don't need to use registers  

Overflow example: char unsigned int = 255 (8bit) for ++ it overflows to 0  
Binary: 11111111  or Hex: 0FF  
32bit register does increment > 100 (Hex) but since char is 8 bits it only takes last 2 digits corresponding to 8 bits. Processor doesn't do extra work thus its 00 in Hex or 0 in 10 base.  

Virtual memory is not the exact memory address but aa virtual one so that other programs cant access for eg. password in another process

Stack, variables added to stack, when function returns to caller, all the stack variables is cleared. Can only delete from top.

CPU to ram distance (8cm approx) , speed of light / 3.2ghz = 8cm. i.e. even a photon cant make a round trip in a single clock cycle. Thus memory is expensive. Optimization ram built into the CPU?  
GHZ  is the unit of a single clock tick tock #todo check this out.  
Latency is the time it takes to make a round trip.  
Throughput is the amount of operation that can be done between first round trip started until the first round trip is finished.  