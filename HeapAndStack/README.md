# Heap and Stack on AVR

## Overview

This page involves memory systems; I revisit this every so often because I live in a world where some ideas that sell well don't live up to there hype.

Which compiler are you talking about, since there are different strategies? If it happens to be GCC for AVR, then the user manual is pretty clear about the layout.

https://www.nongnu.org/avr-libc/user-manual/malloc.html

That is a very informative read; I had no idea that external ram could be used like that.


## Most AVR lack external RAM

The generalized view of memory looks like this:

![AvrHeapAndStack](./Images/AvrHeapAndStack.jpg "AVR Heap and Stack")

The heap and stack can collide if there are sufficient requirements for either dynamic memory system. The former can even happen if the allocations aren't all that large, but dynamic memory allocations get fragmented over time such that new requests don't quite fit into the "holes" of previously freed regions. Large stack space requirements can arise when functions contain large and/or numerous local variables or when recursively calling. Almost no AVR application is mad enough to use the heap (e.g., malloc() and free() ) so the generalized memory use should look like this:

![AvrStackOnly](./Images/AvrStackOnly.jpg "AVR Stack Only")

That is basically how the majority of reliable avr-gcc applications with AVR work.

Globals (e.g., static memory) are positioned at fixed locations in .data and .bss and are placed side-by-side from the start of RAM. The stack is used to preserve registers from ISR's and CALL/RET (e.g., function calls) addresses as well as automatic locals used by the function. All are created on the stack that works its way down from RAMEND downwards (or in the picture leftwards).

Heap needlessly obfuscates how much memory is in use. The compiler can report static memory usage but not dynamic memory usage.  

Partly lifted from this thread:

https://www.avrfreaks.net/forum/what-part-microcontroller-controls-stack-and-heap

## C++

C++ uses the heap, and that is a fact. 

What about automatic garbage collection, doesn't C++ do that? No, it does not. You can write an application that will malloc a block of memory and then garbage collect within that block, but that is a task for the developer. The problem is the features in C++ will allocate outside your garbage collection region. I have come to view the stack as an ideal way to collect garbage; it maintains the free memory as continuous addresses.

So the question I now ask is if you know C++ well enough to avoid using the heap. I do not. I have been told by a few that for them it is perfectly fine. All I can say is that if I can't figure out how to rewrite their perfectly fine library in C, then I walk elsewhere. I have been burned by heap and stack memory corruption enough to know that using C++ is inviting disaster into my project. At some point, I will start looking into how GCC does its heap and stack memory systems on ARM, though at this point I am wagering that the C++ mind virus is just as infectious and sickening as it is on an AVR.

Is it a good idea to teach C++, I think C is simpler to learn so I don't understand the need to teach C++ on AVR (or ARM). I think the Raspberry Pi is an excellent place to learn C++, but they seem to show a lot C (and I support doing that BTW). Somehow the way ideas have played created an upside-down system for learning about computer programing. If I were looking for a gift to give a crafty nephew, it would be an R-Pi, but I worry that it is too powerful. 
