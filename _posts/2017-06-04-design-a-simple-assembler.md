---
layout: post
cover: 'assets/images/cover7.jpg'
title: Designing a simple Assembler
date:   2017-06-04 11:18:00
tags: mcu assembler
subclass: 'post tag-test tag-content'
categories: 'alamgir'
navigation: True
logo: 'assets/images/ghost.png'
---
Following on the design of simple microprocessor in <a href="/design-a-simple-microprocessor.html ">last post</a>, this post talks about writing a simple assembler for that processor. The main aim of this is to learn how the assembly to machine code conversion works. 

<!--more-->

When writing the assembler, in addition to the instructions offered by the MPU, we can add extra dummy instructions, that will map down to the MPU insturctions during assebmly process. For simplicity of programming, I added two instrucions, CMP (meaning complement, functionally same as NOT), and COMP (compare, functionally same as SUB).

We now have a total of 16+2=18 keywords to handle. The Keywords are: <br>

<pre>NOP, STOP, DUMP, LOAD, STOR, XCHG, JMP, JMPE, ADD, 
SUB, INC, DEC, AND, OR, NOT, XOR, CMP, COMP</pre>

And if you look at the opcodes for these instruction mnemonics, the corresponding  machine code should be (in hexadecimal): <br>

<pre>0, 1, 2, 3, 4, 5, 6, 7, 8, 
9, A, B, C, D, E, F, E, 9</pre>

##Assembly process
The assembling process is essentially reading the assembly language program, and going by line by line find the equivalent machine code. In our case, we need to build a parser that could parse all the assembly instructions we allow.

A finite automata (also called state machine) is a perfect tool for modeling parsing. Given the 18 keywords we need to detect, following is an automata that could do that:

![Finite Automata](/assets/images/2017/17_06_04_keyword_automata.png "Finite automata for parsing keywords.") 

How it works is quite easy to understand. Say we have one line of texts from a program written in assesbly language. We begin at the round rectangle block `Begin` and read the first character. We check if it is either A, C, D, I, J, L, N, O, S or X. If the answer is positive, we move to next character, otherwise, the current characte is possibly a white space or a wrong insttuction. All the cicles are intermediary state meaning we have not finished parsing yet. When and if we reach the hexagon, we have successfully identifed or better parsed an instruction. Consider `Begin` as the root of a tree and the hexagons as the leaves. Then following any branch and collecting the label letters will give us only the valid instructions. For instance, from top we have: <br>
<pre>
 ADD
 AND
 CMP
 COMP
 DEC
 DUMP
 INC  etc</pre>
 

##Further Additions
Since the first design of the MPU, there have been few slight modifications, in fact additions, to have better programming capability.  The addtions are: Having a register I (Index register) that will point to the address in data memory where a data will be written to or read from. ADD/SUB/AND/OR/XOR/CMP/NOT can now take register argument. Say, `ADD B` is now a valid instruction that adds the value of B to the resgister A and stores the result in R. Another addition is made to  XCHG that can now take an argument, and based on it, can move values between registers in more flexible ways. For instance, `XCHG $1` will cause the value of A to go to I.

###Example Program
Here is an example program that the assembler can read and assemble into machine code:
<pre>
1  LOAD $0 //load 0 to A
2  XCHG $1   // move 255 to I register
3  LOAD $FF
4  XCHG $0   // move 255 to B
5  LOAD $0   // load 0 into A
6  JMPE $    // if A = B = 255 break loop
7  STOR $55  // store $55 to memory location pointed by I register
8  INC AX    // RX <= A+1
9  XCHG $2   // move back to AX
10 INC IX    // R = I+1 
11 XCHG  $3  // i = i+1
12 JMP $6
13 DUMP $2
14 STOP
</pre>
*Note* Anything after // is comment and is meant for programmer, it is ignored by the assembler. The line numbers at the beginning of each line, again, for ease of reading.

Once the assembler is run on this program, it produces the following output:
<pre>
3200
5201
32FF
5200
3200
7207
4255
A300
5202
A303
5203
6206
2202
1000
</pre>

These machine codes are ready to load the the MPU for execution.

####Source Code
The source code for the simple assembler can be found in <a href="https://github.com/alamgirm/assembler">Github</a>.
