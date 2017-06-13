---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Earliest Microprocessor: Intel 8080' 
date:   2017-06-08 01:18:00
tags: MPU CPU intel 8080
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/logo/logo1.png'
---
The history of what we know as microprocessor (MPU) began with Intel Corporation's 4004 chip, which they built to replace hardwired logic in simple applications. It was a 4-bit processor with very little programming capability which they improved in later 4040 processor. <img src="/assets/images/2017/17_06_08_Intel_8080.png" alt="Intel 8080" class="leftimg" /> Intel followed the development with 8008, and then 8080 both 8-bit processors with better programming support. In fact 8080 was used in a wide range of applications, including the design of few personal computers (PC), and industrial robots. It is the first processor that revolutionized the industry and dictated the future of microprocessors.  Intel 8080 still required extra chips, such as memory, bus controller, clock generator etc to work as a system. This was later improved upon in 8085 that was what we now call system on a chip (SoC)- a chip with built-in everything that requires very little extra to work as a system.

<!--more-->

##Intel 8080
Though 8085 was a much advanced all-in-one chip, it becomes easier if we start our journey of understanding microprocessor generations with 8080. The matter of fact is that 8080 was so popular that there was numerous semiconductor company started manufacturing chips that cloned the functionalities of 8080.

An 8080 is an 8-bit processor, fabricated as a 40-pin chip. It had 8 pins for data I/O, 16 pins for address, 4 control inputs `HOLD, READY, RESET, INT` and 6 control outputs `WAIT, HOLDA, INTE, DBIN, SYNCH, WR`. Looking at the architecture, an 8080 has accumultor, temporary register, instruction register, stack pointer and few `B, C, D, E, H, I` general purpose registers. Some of the registers `BC, DE, HI` can be thought of paired and used as a single 16-bit register when programming. 

![Intel 8080](/assets/images/2017/17_06_08_Intel_8080_arch.png "Intel 8080 CPU architecture.") 

Instructions have different lengths, most are one or two bytes, few are three bytes. The program counter (PC) however knows this fact when incrementing its value to point to the next instruction in memory. Same memory holds both data and program, and maximum of 64KB is supported. However, 8080 also supports what is called I/O address space. That means, I/O devices could be attached to the data and address bus, (in conjunction with `READY, WAIT, HOLD, HOLDA`) without sharing the memory address space. A total of 256 input and 256 output ports are supported; each port has 8-bit parallel data transfer capability.

Interrupts are supported via `INT` (interrupt request) pin. However, it is software maskable (can be enabled/disabled) using `EI/DI` instructions. When an intrrupt occurs, the CPU acknowledes by enabling `INTE` pin. For storing the addresses of interrupt service routines, first 64 bytes in memory are reserved.


###Support Chips
A full-blown system with Intel 8080 required few support chips. Among them these were popular:

- 8238 System controller and bus driver
- 8251 UART (universal asynchronous/synchronous receiver/transmitter)
- 8253 programmable interval timer/counter
- 8255 programmable peripheral device
- 8257 DMA (direct memory access) controller
- 8259 interrupt controller


####Credits
- The 8080 image is borrowed from <a href="http://www.wikiwand.com/de/Intel_8080">WikiWand</a>
- The 8080 architecture image is taken from <a href="http://expaworld.blogspot.com">Blogspot</a>
