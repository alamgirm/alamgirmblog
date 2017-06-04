---
layout: post
cover: 'assets/images/cover7.jpg'
title: The Microprocessor, microcontroller and programming
date:   2017-06-03 15:18:00
tags: mcu embedded
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
A microprocessor, or microprossor unit (MPU) often also known as central processing unit (CPU) is generally a physical semiconductor chip with lots of pins to connect to other chips, that can execute certain instructions. Here execution means, when certain electrical signals are presented at the inputs of the MPU, is will produces some signals at its output pins. Almost always, a MPU is connected to a memory chip (via two groups of wires known as data bus and address bus plus some more control wires) where the instructions (combinedly known as program) are stored. A MPU will always have a clock that coordinates the transfer of signals between MPU and different chips. Other than memory, some form of input and output device are also often connected to an MPU. These are commonly connect to the MPU via some controller chip. Here is an example of a simple system involving all the things mentioned above.
![BareBone MPU](/assets/images/2017/17_06_03_image_1.png "MPU, Memory, Keypad and LCD display.") 

Before anyone could write any program these components need to be properly connected with wires and supplied power. Even after that, programs written, will get lost when the power is removed.
<!--more-->

Building a system this way is harder than it now needs to be. Welcome microcontroller unit (MCU), also called system-on-a-chip (SoC), that combines one or more processing core, both volatile and no-volatile memory, analog/digital converter, timers, counters and heaps of other usefull components- all packaged ina single chip. All is needed to connect to outside world via input/output components.

Depending on the manufacturer and product line, MCUs can have numerous capabilities built on to it. Intel 8051, a historically popular MCU has 8-bit processing element, 128-byte of RAM, 4KB of ROM, 4 sets of 8-bit programmable I/O, a serial commmunication port, two 16-bit counter/timer etc.

Neumann vs Harvard, RISC vs CISC: Instruction Set
-------------------------------------------------
A MPU is designed with certain instrtuctions in mind, and even before that certain architecture in mind. Two architectures are in wide use: Von Neumann and Harvard. On the surface, a Von Numann MPU will access only a single space of memory, both for program instructins the data the program uses. In a Harvard architecture program and data are considered separate spaces- and there are certain advantages for that in expense of added complexity.

Talking about complexity, a MPU can be designed with instructions that try to go the long way, or stay as simple as possible. Take for example, multiplying two numbers. An MPU can have an instruction called MUL to do just that, or lack one and expect the programmer to do multiplication using ADD instruction. ADD instruction is very simple for implementation. This division in philosophy resulted in CISC (complex instruction set computer) and RISC (reduced instruction set computer). RISC processors are fast, since they have simple architecture and limited set of instructions, but as we just learned, needs to execute more instructions to get something done. CISC on the other can have big instruction set to get things done quickly, but could eventually be slower.  

Development board, IoT
----------------------
A MCU though comes with lots of things in-built still needs power, ways to connect to input/output components and hence needs to be in the form of a development board- a circuit board with the MCU and other relaeted circuitry to provide power, clock, headers for connection etc. Luckily there are huge range of development boards are avaialble: Arduino, Raspberri Pi, Orange Pi, Banana Pi, BeagleBone, and the list goes on. Apart from the capabilities offered by the MCU used in these boards, some boards have added features, ways to connect more add-on feature (often called shield), and sometimes extra ports to make it easy to connect to say USB, HDMI, WiFi, Ethernet etc.

Programming a dev-board
---------------------------
All MCU have some kind of non-volatile memory to store programs and volatile (RAM) memory for data. But  for putting the program into the memory needs assistance from the MCU. A ROM (EEPROM) is often is there to do that. A small program, called bootloader who sole purpose is to help load user programs into the memory, resides in the ROM. The bootloaded is however replacable via special hardware. Two such hardware are: ICE (in circuit emulator), and JTAG (joint test action group). The MCU in use must support such hardware feature, and the dev-board should also have connector slot available. Other than replacing the bootloader ICE and JTAG device can debug programs and do lot more, for example change internal MCU flags that the MCU use for its configurations.

What's Next
-----------------
-Program organization in detail
-Bootloader in detail
