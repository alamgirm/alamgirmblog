---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: Microprocessor, microcontroller and programming
date:   2017-06-05 00:18:00
tags: [MCU, Embedded Programming]
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
A microprocessor, or microprossor unit (MPU) often also known as central processing unit (CPU) is generally a physical semiconductor chip with lots of pins to connect to other chips. But what makes it special is the capability that it can execute certain instructions. Here execution means, when some electrical signals are presented at the inputs of the MPU, is produces some appropriate signals at its output pins. Almost always, a MPU is connected to a memory chip (via two groups of wires known as data bus and address bus plus some more control wires) where the instructions (combinedly known as program) are stored. A MPU will always have a clock that coordinates the transfer of signals between MPU and different chips. Other than memory, some form of input and output devices are also usually connected to an MPU. These are commonly connect to the MPU via some controller chip. Here is an example of all the things mentioned above.
![BareBone MPU](/assets/images/2017/17_06_05_image_1.png "MPU, Memory, Keypad and LCD display.") 

Before anyone could write any program these components need to be properly connected with wires and supplied power. Once powerd programs can be written into the memory and exucuted. However, both programs and data will get lost when the power is removed since the memory shown is a volatile type one.
<!--more-->

Building a system this way is harder than it now needs to be. Welcome microcontroller unit (MCU), also called system-on-a-chip (SoC), that combines one or more processing core, both volatile and no-volatile memory, analog/digital converter, timers, counters and heaps of other usefull components- all packaged in a single chip. What left is to connect the chip to outside world via input/output components.

Depending on the manufacturer and product line, MCUs can have numerous capabilities built on to it. Intel 8051, a historically popular MCU has 8-bit processing element, 128-byte of RAM, 4KB of ROM, 4 sets of 8-bit programmable I/O, a serial commmunication port, two 16-bit counter/timer etc.

#Neumann vs Harvard, RISC vs CISC: Instruction Set
An MPU is designed with certain instrtuctions in mind, and even before that certain architecture in mind. Two architectures are in wide use: Von Neumann and Harvard. On the surface, a Von Numann MPU will access only a single space of memory, both for program instructions and the data that the program uses. In contrast, in a Harvard architecture two spaces of memories are used, program memory and data memory. A program can not modify anything in the program memory. Data are always read/written from/to data memory. There are certain advantages for doing these in expense of added complexity.

Talking about complexity, a MPU can be designed with instructions that try to go the extra mile, or stay as simple as possible. Take for example, multiplying two numbers. An MPU can have an instruction called MUL to do just that, or lack one and expect the programmer to do multiplication using ADD instruction. ADD instruction is trivial for implementation.

This division in philosophy resulted in RISC (reduced instruction set computer) and CISC (complex instruction set computer). RISC processors are fast, since they have simple architecture and limited set of simple instructions, but as we just learned, needs to execute more instructions to get something done. CISC on the other can have more instructions available to get things done easily from programmer's points of view, but could eventually be slower.  

#Development board, IoT
An MCU though comes with lots of things in-built still needs power, ways to connect to input/output components and hence needs to be in the form of a development board- a circuit board with the MCU and other relaeted circuitry to provide power, clock, headers for connection, means to load program etc. Luckily there are huge range of development boards avaialble: Arduino, Raspberri Pi, Orange Pi, Banana Pi, BeagleBone, and the list goes on. Apart from the capabilities offered by the MCU used in these boards, some boards have added features, ways to connect more add-on feature (often called shield), and sometimes extra ports to make it easy to connect to say USB, HDMI, WiFi, Ethernet etc.

#Programming a dev-board
All MCU have some kind of non-volatile memory to store programs and volatile (RAM) memory for data. But  for putting the program into the memory needs assistance from the MCU. A ROM (EEPROM) is often is there to do that. A small program, called the bootloader whose sole purpose is to help load user programs into the memory, resides in the ROM. The bootloader is however most often replacable via special hardware. Two such hardware are: ICE (in circuit emulator), and JTAG (joint test action group). The MCU in use must support such hardware feature, and the dev-board should also have connector slot available. Other than replacing the bootloader ICE and JTAG device can aid debug programs and do lot more, for example change internal MCU flags that the MCU use for its configurations.

###What's Next
- Program organization in detail
- Bootloader in detail
