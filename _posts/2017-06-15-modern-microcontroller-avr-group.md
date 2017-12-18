---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Modern microcontroller: AVR series' 
date:   2017-06-13 06:18:00
tags: [MCU, Atmel AVR, Arduino]
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
The story of AVR familiies of microcontrollers (MCU), that power popular Arduino development boards began with two Norwegian students who designed a microcontroller as academic project. It was based on modified Harvard achitecture and RISC instructions set. The design later bought up by Atmel, and the two original designers continued to work on and improve. <img src="/assets/images/2017/17_06_13_Atmel_AVR_logo.png"  alt="AVR logo" class="rightimg" /> Atmel, understandly targetting the market of Intel 8051 releases the first AVR AT90S8515, a 8-bit MCU with same pin out as Intel 8051. Though originally 8-bit today AVR has 32-bit MCU, and offers multiple families of MCU with varied degree of capabilities and functionalities. There are however few common features: for example use of on-chip flash memory for program storage, EEPROM bits for CPU configuration flags etc. Within each family there are multiple MCUs available.
 
<!--more-->

##AVR Families
AVR MCUs are generally put into any of the following falimies:

| Feature        | TinyAVR      | MegaAVR     | XMEGA    | AVR32  | App Specific|
| ---------------|--------------|-------------|----------|--------|-------------|
| Data memory    | uptp 1024    |             |          |        |             |
| Word size      | 8-bit 		| 8-bit       | 8-bit    | 32-bit | depends     |
| Program memory | 0.5 to 16KB  | 4-256KB     | 16-384KB |        |             |
| Package        | 6-32 pin     | 28-100pin   | variable |        |             |
| Peripheral     | limited      | extened     | extended |        |             |
| Instrunctions  | Some limit   | Extened     | Extened  |        |             |
| Extra feature  | No	        | Option      | Option   |        |             |


##Specific Models
Below are some specifuc models that are used often. The table is not complete, and accuracy is not guranteed.

|			  | Flash memory | EEPROM | SRAM | Chip pins | I/O pins | Clock | Feature |
|-------------|--------------|--------|------|-----------|----------|-------|---------|
| ATtiny13    | 1K			 | 64	  | 160	 | 8		 | 6		| 20MHz	| 		|
| ATtiny85    | 8K			 | 512	  | 512  | 8		 | 6		| 20MHz	|		|
| ATMega32    | 32K			 | 1K	  | 2K   | 40		 | 32		| 16MHz	|		|
| ATmega32U4  | 32K			 |		  |      | 44 		 | 26		| 16MHz	| USB	|
| ATmega128   | 128K		 |		  |      | 64		 | 			|		|		|
| ATmega168   | 16K			 | 512	  | 1K   | 28/32	 | 23		| 20MHz	|		|
| ATmega328   | 32K			 | 1K	  | 2K   | 28/32	 | 23 		| 20MHz	|		|
| ATmega649   | 64K			 |		  |		 | 64	 	 | 			|		| LSB	|
| ATmega1280  | 128K		 |	      |		 | 100	 	 | 86		| 16MHz	|		|
| ATmega2560  | 256K		 | 	      |		 | 100		 | 86		| 16MHz	|		|


###More details

More details about the features of various families can be found in <a href="http://www.atmel.com/products/microcontrollers/avr/default.aspx">Atmel's web</a> site. Considering both the price and features, ATMega family is probably the best choice among IoT and hooby users. Choices and details about ATMega family chips are available <a href="http://www.atmel.com/products/microcontrollers/avr/megaavr.aspx"> here.</a>