---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Serial Communications- How to' 
date:   2017-07-08 13:18:00
tags: Serial Communication RS232
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/logo/logo1.png'
---
<img src="/assets/images/2017/2017_07_08_RS232.png"  alt="RS232" class="leftimg" /> Digital systems need to communicate, even devices within a single system need to communicate with each other. In hardware, this means exachanges of digital signals between two parties. Since data are represented as binary numbers in a digital system, transferring of a binary number can happen in two manners: all the bits in one go (in parallel) or one bit at a time (in serial). This gives rise to design of two types of communication system: parallel and serial. The first thing to note is parallel communication needs more wires but supposedly transfer data in relatively faster manner. In contrast, serial communication can work with fewer wires, but would take longer to transfer the same amount of data. From this, it might look that parallel communication is 'better' than serial one, but in reality this is hardly the case.

<!--more-->

##Role of Clock: synchronous vs asyncrhonous
When two parties communicate, it is very important that they are on the same page, i.e, the receiver understands what the sender is sending. In Digital communication, at the hardwire level, the reciver must know when a bit starts, how long is the duration etc. One very common way to keep both the parties in synch is to use a series of rectangular pulse, known as clcok signal, clock pulse, or simply clock. Generally the sender party is responsible for sending the clock and the receiver gets in synch to it, before trying to detect the data. This is syncrhonous communication. While this sounds good, there are more problems with sending clock: they get smeared over distance, and suffer from jitter. The problems are sometimes so bad, that it is now very common not use any separete clock at all. Rather, the data signal itself has built-in clock signal. In this case, the reciever runs its own clock, but aligns it when a wave of data comes in. This is asynchrounous communication. 


###Line-coding of Data
We know data are represented in Binary in digital systems, and often a bianry 1 means HIGH voltage (3.3V or 5V) and 0 means LOW voltage (0V). However, it is not uncommon to use a *negative voltgate to indicate 1*. In addition,  when we send these data over a communication line, there is one more thing: the duration of each bit. The receiver must know how long a bit lasts. Bits are sent one after another without any gap in between, so the receiver must also know when it is to start, when it should stop. 

In one particluar instance (RS232- see later) of serial communication, few bits of Binary data are sent as a frame. A frame is a single unit, the receive either receives it fully as good or discards it. The start of frame is indicated by a LOW pulse, followed by the data bits. A parity bit is then added for error checking, followed by one or two stop bits (HIGH) indicating the end of frame. The duration of each data bit, (inversion of which called baud rate), number of data bits, type of parity and number of stop bits- these are all configurable parameters. The hardwire chip (often called UART chip)  expects these parameters before a transmission could begin. UART chips are dumb in the sense that they do not have the capability to negotiate the parameters with other party, it depends on the programmer to set the parameters.
<img src="/assets/images/2017/2017_07_08_linecoding.png"  alt="RS232 Line coding"  />

This little picture needs some explanation. The start bit is ALWAYS low, it designates the start of the frame. So as the UART on the receiving end while monitoring its RX line, can start to detect data bits the moment RX goes LOW. The next few pulses can be HIGH or LOW or a mixture, depedning on exactly what the data bits are. This is the greay area in the picture. To help check with error detection the sender party may add an extra parity bit. It could be either LOW or HIGH depending on the party type in use, or even omitted if no parity is used. This decision is made by the developer, when configuring the UARTs. Finally the stop bit(s) are always HIGH, the red area in the picure. The duration can be double of the start bit if two stop bits are required.

###RS232
RS232 is a very old stadard of serial communication, long before modern day PCs have come. It evolved over time, came down to 9-pin from original 25-pin connector, and now even bare 3-pin/line communication is quite common.  At this bare minimum, we have a TX (transmit), RX (receive), and a GND (ground) line. TX and RX are crossed bewtween the devies that communicate, so that either of the device can send data to another. Data can flow full-duplex, meaning both parties can send/recieve simultaneously (as they use different physical lines).

In embedded development a serial link (not exactly an RS232) is often used to either program a board or connect a serial display terminal. Recent PCs dont have any RS232 port any more, so a USB to Serial converter is used. The original RS232 stipulated a +3V to +15V for representing a Binary 0 and -3V to -15V for a Binary 1. For the serial communication on embedded boards, and popular IoT baords, the voltage level is reversed and reduced. A Binary 0 is often 0V and a Binary 1, is either 3.3V or 5V. This is *opposite* of what the RS232 standard uses. To avoid confusion, some people refer to this as *TTL Serial*. It is to remember that the start bit is still a LOW pulse, and the stop bit(s) is HIGH.

The baud rate is standardized to any of 1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. The rate 9600 is the most common, some using 115200 for short cable. The *TTL Serial* adheres to the same standard baud rates.

###Configuration Example
If we want the baud rate of 9600, 8-bits of data, no parity, and just one stop bit, then each frame would have 10-bits. The extra two bits (start and stop) are overhead, but we have to use them to make the communication happen.

####Terminal Emulator
Finally there is a software tool that can emulate a serial terminal. This when connected to an embedded board helps us to see what is going on. On Linux platforms `picocom` and `minicom` are two popular serial terminal emulator. Again, before they could display something meaningful, they need to be configured with correct baud rate, data bits, parity and stop bits. 

###Common Mistakes

- The configuration parameters: baud rate, data length, parity and stop bit must be set at the *BOTH* end of the link. Both the UART must have the same parameters to make the communication workable.
- The TX and RX lines *MUST* be crossed- othewise there is no chance of communication.
- Some boards are powered by 3.3V supply, and cant withstand any voltage higher than that. Make sure the USB to Serial converter does not have 5V in such case.
- Serial communication is point-to-point or party to party. It can have more than two devices. If more than 2 devices are connected on the same wires, it wont work.
