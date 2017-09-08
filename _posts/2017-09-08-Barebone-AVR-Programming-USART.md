---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: 'Barebone AVR Programming - USART' 
date:   2017-08-09 20:18:00
tags: AVR barebone Amtel
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/logo/logo1.png'
---

<img src="/assets/images/2017/2017_08_09_AVR_board.png"  alt="AVR Board" class="leftimg" />
Continuing on from last <a href="Barebone-AVR-Programming-LED-Blinker.html" >post</a> this blog post talks about using the serial communication function on AVR, specifically the USART for communicating with other party. The same development board is used.
<!--more-->

###Setup
The development board is programmed to send a string out of USART. These data are sent out via some of the GPIO pins. As a receiver I used a terminal emulator programming on Mac OSX. A USB-Serial converter is used so that the serial data can be received through the USB port on Mac (as it didnt have any serial port). The USB-Serial converter I used is obtained from <a href="https://www.aliexpress.com/item/1PCS-PL2303-TA-USB-TTL-RS232-Convert-Serial-Cable-PL2303TA-Compatible-with-Win8-Win10/32775876897.html">Aliexpress</a> but it essentially a clone of what <a href="https://www.adafruit.com/product/954">Adafruit</a> has. The converter has 4 wires: Black (Ground), Red (+5V supply), Green (TX, out of USB), and White (RX, into USB).

The board I have uses an ATMega128 chip which has two USARTs. USART0 has its I/O available on Port E, more specifically after looking tha ATMega128 datasheet (18.3.5): PE1 is UART0 transmit pin, PE0 is UART0 receive pin. Since USART is point-to-point communication we must cross the TX-RX wires when connecting two parties. In our case, the Green (TX) wire goes to PE0 (USART0 receive pin) and the White (RX) wire goes into PE1 (USART0 transmit pin). Technically, we dont need to connect the white wire in this particular case since we are not transferring any data from the terminal emulator to the AVR development board. 
 
On Mac OSX we used the program called picocom to receive the communications. To have a successful communication both paties must have same baud rate, data bits and stop bits. picocom is run with 9600 baud, 8 data and 2 stop bits.

###Programming setup  
I write a simple program in C language. The program frist setsup the USART for baud rate, data bits and stop bits. If you are unsure of what they are read earlier post on serial communication.  Once the USART is configured we send a string out of USART0. After a little delay the message is sent again in a loop. AVR Studio 4 is used to compile, and JTAG programmer is used to upload the code to the board.

<pre>
	//
	//	File name: TestUSART.c
	//	Purpose: Send a string out of USART and have a terminal  
	//           software reading it.
	//  Credit : The configuration and data sending functions are copied from ATMega128's data sheet.
	
	#define F_OSC 8000000 // clock speed, set via Fuse bit
	#define F_CPU F_OSC   // needed for the delay functions to work

	#include <avr/io.h>
	#include <util/delay.h>

	#define BAUD_RATE 9600
	#define MYUBRR  F_OSC/16/BAUD_RATE-1


	//redefine the regs
	#define 	UCSRA 	UCSR0A
	#define 	UCSRB 	UCSR0B
	#define 	UCSRC 	UCSR0C
	#define 	UBRRH 	UBRR0H
	#define 	UBRRL 	UBRR0L
	#define 	UDRE 	UDRE0
	#define 	UDR 	UDR0
	#define 	RXC 	RXC0

	const char *str = "This is a message from the board";


	void USART_Init(unsigned int ubrr);
	void USART_Tx(unsigned char data);

	int main (void) 
	{
	
		USART_Init(MYUBRR); // configure the USART
		_delay_ms(1);       // delay a bit


		while(1) {
			USART_Tx_Str(str); // send out the message
			_delay_ms(4000);   // wait for 4 seconds
		}
	}

	void USART_Init(unsigned int ubrr)
	{
		// set baud rate
		UBRRH = (unsigned char) ( ubrr >> 8);
		UBRRL = (unsigned char) ubrr;
		//enable rx and tx
		UCSRB = (1 << RXEN) | ( 1 << TXEN);
		//set frame format: 8-bit data, 2 stop bit
		UCSRC = ( 1 << USBS) | ( 3 << UCSZ0);
	}


	void USART_Tx(unsigned char data)
	{
		//wait while the transmit buffer is not empty
		while(! (UCSRA & (1 << UDRE)) );

		//out the data on buffer
		UDR = data;
	}

	void USART_Tx_Str(char *str)
	{
		int i;

		for (i=0; i < strlen(str); i++)
			USART_Tx(str[i]);

		// We are on a at the receiver end
		// So we need \r\n for new line 	
		USART_Tx('\r');
		USART_Tx('\n');
	}
</pre>

###Explanation
The C program code first defines the processor clock frequncy via `F_CPU`. This is required for the delay functions used in the program. We then define the baud rate of the communication, and based on that need to define the parameter UBBR. A handful of macros are then defined agains the 0th USART. If you want to use USART1, change all those 0 to 1.

The program first initializes USART0 for the desired baud rate. Then it enables both TX and RX mode. Finally frame format is chosen: 8-bit data, 2 stop bit. 

When sending out the message string, each character is sent out in serial. The sending function must check if the transmit buffer is empty before sending out a character.

Finally as we are using a termian program on Mac OSX we need to send additional carriage return + new line character so that multiple strings appear one below the other.  

###Output
A short clip showing the message <a href="/assets/images/2017/2017_09_09_USART.mp4">being sent via USART</a>.

###Links to software and hardware

- USB-Serial <a href="https://www.aliexpress.com/item/1PCS-PL2303-TA-USB-TTL-RS232-Convert-Serial-Cable-PL2303TA-Compatible-with-Win8-Win10/32775876897.html"> converter/cable </a>
- The <a href="https://www.aliexpress.com/item/1PCS-DC-5V-ATmega128-AVR-Core-Development-Board-Minimum-System-Module-ISP-JTAG/32745789402.html">development board</a>. 
- JTAG <a href="https://www.aliexpress.com/item/Free-shipping-AVR-USB-Emulator-debugger-programmer-JTAG-ICE-for-Atmel/623898152.html">debugger/programmer</a>.
- How to install <a href="http://automatica.com.au/2011/09/picocom-1-6-for-mac-osx-10-6/">picocom on Mac OSX</a>