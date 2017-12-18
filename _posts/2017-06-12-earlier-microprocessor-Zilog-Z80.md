---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Earliest Microprocessor: Zilog Z80' 
date:   2017-06-12 06:18:00
tags: [MPU, CPU, Zilog Z80]
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
The CPU design team at Intel in the 70's was led by <a href="https://en.wikipedia.org/wiki/Federico_Faggin">Federico Faggin</a>. After resigning from Intel Federico formed the start-up company Zilog, for design, development and marketing of microprocessor. <img src="/assets/images/2017/17_06_12_Zilog_Z80.jpg"  alt="Zilog Z80" class="rightimg" /> Zilog Z80 was their first product to hit the market, aimed for embedded applications. Its direct competitor was Intel 8080, and in fact Zilog always highligheted in commercials about its superiority over 8080. Compared to Intel 8080 which required +12V, -5V and +5V, Z80 needed only one +5V supply, had simple clock, and supported more registers, instructions, addressing modes, and even non-maskable interrupts.   Z80 was a gigantic success that helped the company grew from just elevel to a thousand employees in just two years. Many other semiconductor companies around the world copied the design of Z80 and sold their chips. Famaous Atari computuers, the first one to have color display were built on Zilog Z80.

<!--more-->

##Zilog Z80
Looking at the internal block diagram, Zilog Z80 has similar architecture as compared to Intel 8080, same 8-bit data bus and registers, and 16-bit address bus. What is surprising is the inclusion of two sets of general purpose registers. Namely, the registers `W, Z, B, C, D, E, H, L` can used in pairs to act like 16-bit as with 8080, but their contents can be saved by copying to a whole set of mirrored registers named `W', Z', B', C', D', E', H', L'`- and this could be done using just one EXX instruction. The accumulator `A` has also a friend `A'` also do the flags set `F` as `F'`. These primed registers could not be loaded directly, only could have swapped values with their counterparts. The special purpose registers are similar, bearing similar names too: PC (program counter), SP (stack pointer), IX and IY (both index registers). This feature of having two sets of registers make programming much easier when writing sub-routine or interrupt service.

![Zilog 80](/assets/images/2017/17_06_12_Zilog_Z80_arch.png "Zilog Z80 CPU architecture.") 

Zilog Z80 supported maximum of 64KB memory, and additional 256 I/O ports each. Dymanic memories could be used without specilized refresh circuit as the CPU itself provides one.

Similar to Intel 8080, Zilog Z80 overlaps its fetching and executing cycle, meaning, while an instruction is being executed, the next instruction is fetched from memory if the bus is is not being used. Most of the instructions for Z80 are just one byte, but the longest one can be 4-byte long. The total number of instructions is also higher than Intel 8080.

###Support Chips
A full-blown system with Zilog Z80 could be built without needing much extra chips. However, many of the following chips that were originally designed to work with Intel 8080 work just fine with Zilog Z80.

- 8238 System controller and bus driver
- 8251 UART (universal asynchronous/synchronous receiver/transmitter)
- 8253 programmable interval timer/counter
- 8255 programmable peripheral device
- 8257 DMA (direct memory access) controller
- 8259 interrupt controller


####Credits
- The Zilog Z80 image is courtesy of <a href="http://www.cpu-world.com/CPUs/Z80/">CPU-World.com</a>
- The architecture image is borrowed from <a href="http://www.z80.info/z80arki.htm">Z80 Info</a>