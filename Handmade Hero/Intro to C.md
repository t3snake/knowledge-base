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

Endian > Big and little is just ordering of bits/bytes.  
Modern ones are all little endians where lower order bytes are filled first and then higher order bytes. for eg unsigned int = 500 would be 256 + 244 or 00000001 11110100 which in memory is 244 and 1.  

Ps3 Xbox 360 and older were big endian, ps4 onwards is little. Old formats might be in big endian > might need to convert into little endian to read.

Structs may use different blocks of memory instead of next to each other.

Stack, variables added to stack, when function returns to caller, all the stack variables is cleared. Can only delete from top.

CPU to ram distance (8cm approx) , speed of light / 3.2ghz = 8cm. i.e. even a photon cant make a round trip in a single clock cycle. Thus memory is expensive. Optimization ram built into the CPU?  
GHZ  is the unit of a single clock tick tock #todo check this out.  
Latency is the time it takes to make a round trip.  
Throughput or Bandwidth is the amount of operation that can be done between first round trip started until the first round trip is finished. Or in peak traffic how many operations can I start in a second.  

In CPU, L1 cache is closest, then L2 and L3 which are pieces of memory on CPU to reduce round trips to memory.

Even though pointers are size 4 ie stores address, C needs to know what it is pinting at so type matters but we can cast to 1byte pointer for a 16 byte type and then work on single bytes.

Arrays are basically pointers. int array\[30] -> no need to do &array, array points to the first index automatically.  
In c when we do `(ptr + 30)->member`, it basically already converts to `ptr + 30 * sizeof(ptr)`
If we want to do manual we might need to do `(ptr_type * ) ( (char * )ptr + 30 * sizeof(ptr))` to basically do what `(ptr + 30)` does. Cast to `char *` to work in single bytes.  

if not pointer `ABC.member`, if pointer `ABC->member`. There is no other special difference.

In debug mode visual studio puts in 204 in all unassigned/garbage memory. Its basically 0xCC in hex which dev can quickly see that it is uninitialized. In production mode, compiler wont spend effort trying to do that.  

## Bitwise operators

### Shift
> \>> or <<

    x = 1   // 0001  
	x = x << 1  // 0010  

in hexadecimal 

    x = 10    // 0x00000a  
    x = x << 4    // 0x0000a0  

1 digit in hex is 4 digits in binary << and >> work with binary digits
In C by default if shifted out of range, the bit is lost and cant be recovered.  
> **Note**: whatever we do a **<< 1** on will be doubled and **>> 1** will be halved (if no bits lost).
> **<< 2** will quadruple (2 pow 2) it, **<< 3** will multiply 8 and so on...

### Or
> |

Basically an or operator.

	x = (1 << 4) | (1 << 8) // 0x0010 or 0x0100 = 0x0110

### And
> &

Basically an and operator.  

	x = (1 << 4) | (1 << 8) | (1) // 0x0111
	y = (1 << 4) | (1 << 12)      // 0x1010
	x = x & y                     // 0x0010

### Xor
> ^

Basically exclusive all operator

	x = (1 << 4) | (1 << 8)   // 0x00110
	y = (1 << 8) | (1 << 16)  // 0x10100
	x = x ^ y                 // 0x10010
	x = x ^ y                 // 0x00110 we get back original x

We can xor with the same key twice to get back original value.   

