---
layout: post
cover: 'assets/images/cover/cover7.jpg'
title: Understanding Program Flow and Interrupts
date:   2017-06-05 17:18:00
tags: [MCU, Interrupt]
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
Once we understand how a microprocessor works, and how programs are written and get executed by a microprocessor, it is time to dig deeper and learn in detail on how the control in a program flows between instruction to instruction. This posts talks about the flow or path of execution in a program, and how and when the microprocessor could break that flow. In particular, we'll learn about interrupts- a very useful mechanism to allow priority and importance in program execution. 

<!--more-->

##Control Flow
Lets begin with an example program in assembly language:
<pre>
0  LOAD $8    // load 8 to A, initialize A
1  XCHG $0    // BX <= AX, move the value of A to B
2  LOAD $10   // load 10 to A, initialize A
3  DEC  AX    // R <= AX -1, decrement A
4  XCHG $3    // AX <= R, move the result back to A
5  JMPE $7    // if A = B jump to instruction at $7
6  JMP  $3    // jump back to $3, continue loop
7  DUMP $2    // dump values
8  STOP       // stop the processor
</pre>

The number on the left denotes the physical memory address of each program statement. The program begins with loading $8 to register B (by first loading it into A, then exchanging), then loads $10 to register A.  The next two statements decrease the value of A by 1. After that, we compare the values of registers A and B and if equal, break the loop by jumping out of it to address $7. If the values are not equal, the statement `JMP $3` takes us back to the decremnt statement at address $3.

If we have a highlighter to point to the current line (the one being executed by the MPU), then these will be the sequecnce of instructions as executed by the processor:
<pre>
	0
	1		// A now has 8
	2		// B now has 8 (A also has 8)
	3		// A now has 10, B has 8
	4		// A has 10, R has 9, B has 8
	5		// A has 9, B has 8
	6		// A has 9, B has 8
	3		// A has 9, B has 8
	4		// A has 9, R has 8, B has 8
	5		// A has 8, B has 8
	7		// A has 8, B has 8
	8		// microprocessor halts
</pre>
If we notice carefully, the statements at addresses `3,4,5` get executed twice. After first round, the processor executes `6` then goes back to `3`, and on the second round the microprocessor skips the statement at `6` and jumps to `7`. The flow of control (the statement that is being executed) here is definetely not linear. Well, it is linear unless not broken by a JMP/JMPE instruction.

##Sub-routine or Function
Repeating a group of statements is called looping in high-level languages. Another feature almost all high-level languages offer is called sub-routine or function. The idea behind sub-routine/function is to write a small program to perform a certain specific task and then reuse it as many times as possible, without rewriting it. So, for example, we could wirte a few statements to find the bigger number from a pair of given numbers. We could expect that the numbers to be compared be given in register A and B, and the result will be provided in register R. Now lets assume these group of statements (called subroutine/function now) be placed at memory address $12.

Now when writng our main program, and when in need of finding the bigger of two numbers, we would just load the numbers in A and B, and then *call* the subroutine we just created before. In fact, we'd have to make a JUMP to address $12. And once that is done, we will have to come back right where we left off before making the jump.

Here is a demo example.
<pre>
	 0  LOAD $8    // load 8 to A, initialize
	 1  XCHG $0    // BX <= AX, move the value of A to B
	 2  LOAD $10   // load 10 to A, initialize A
	 3  JMP  $2F   // call the subroutine
	 4  XCHG $3    // call retruns here, move the result R to A
	 5  DUMP $2    // dump values to show result
	 6  JUMP $1F   // jump to the end of program
	...................................................
	
	12 SUB BX      // R <= AX - BX
	13 ------	   // if R has Sign Flag on
	14 ------	   // B is bigger
	15 ------	   // otherwise, A is bigger
	16 JMP $4      // return to caller routine
	...................................................
	 			
	1F  STOP       // stop the processor
</pre> 
What goes inside the subroutine is ommitted here for simplicity, and it was assumed the subroutine knows the program address $4 (where to return to). And, it was assumed the MPU does not have any special instructions specifically designed to support function call. In practice, real MPUs do have instructions to make function call easier.

Use of subroutine/function is no different to the MPU, it executes the statements in the same manner, as it does the main program. The only benefit is to the programmer, who can better organize the program when writing.

##Interrupts
The simple microprocessor we <a href="/design-a-simple-microprocessor.html ">designed</a> does not support any kind of priority or urgency. For instance, if the microprocessor is busy running a long loop, there is no means for the programmer to stop or cancel the loop without resetting or powering off the MPU. We ccould add a register named NR (iNterrup Register) in the design, and a hardware pin named IntReq (Interrupt Request). The idea is to put an address (of the starting statemnt of a group of instruction or subroutine)  in NR, and make the MPU jump that address whenever the pin IntReq becomes active. Once that subroutne is done, the MPU will come back where it left off before the IntReq pin became active. Making the IntReq pin active is known as interrupt request, and the subroutine that executes in response to this request is called *interrup service routine* (ISR).

Interrupts are very common in commercial MPU, and in fact most MPU support more than one interrupt. It is also common to extend the interrupt request feature so that more than one request could be handled. For instance, our MPU could set aside 8-bit x 16 memory, where 16 different memory addresses would be stored. Before making an interrupt request, a device first put its identifier (say betweet 0 and F) into a pre-designated register or memory address, and then make IntReq active. The interrupt service will first see the id of requesting device and then based on it, choose the corresponding entry of the 8-bit x 16 memory table. No wonder, such table is known as *interrupt vector table* since each entry of the table is an address (vector) of the subroutine corresponding to an interrup request.

While interrupts are great, it makes the control flow of program unpredictable. It is not possible beforehand which interrup will go active and when. Some MPUs, also prioritize interrupts meaning some interrupts are more important than others.