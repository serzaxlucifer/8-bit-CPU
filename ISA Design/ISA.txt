# Instruction Set Architecture

Prepared by Mukul Malik (21ucs133)

Please note the following:
1. This ISA design is not final and might change while designing the actual CPU circuits, components and modules in coming weeks/labs if we choose to change the instruction length, addressing modes, number of registers or bus sizes. This is an initial draft with a circuit in mind.

We’re using 4 8-bit registers named as R0, R1, R2, R3.
Register Name	  Register Address
R0	            00
R1	            01
R2	            10
R3	            11

We’re using a main memory of 256 bytes which is byte addressable.
Instruction Length: 8 bits/1 byte
Number of Registers: 4
Main Memory: 256 bytes
Size of Bus: 8 bits
INSTRUCTION FORMAT:
0	1	2	3	4	5	6	7

0-3 will specify the OPCODE.
We’re only using register-register addressing. So,
4-5 and 6-7 will specify Reg A, Reg B (RA, RB) respectively.
REGARDING OPCODES:
1.	ALU Operations: If the 0th bit is “1”, that will imply that it is an ALU Operation. Then the next 3 bits will determine the type of ALU Operation.
ALU OPERATION	1st bit	2nd bit	3rd bit
ADD	0	0	0
SHR (Shift Right)	0	0	1
SHL (Shift Left)	0	1	0
NOT	0	1	1
AND	1	0	0
OR	1	0	1
XOR	1	1	0
CMP 	1	1	1

2.	Load/Store Instructions: If the 0th, 1st and 2nd bits of the Opcode are 0, then it is a Load/Store Instruction. If the 3rd bit is 0, then it is a LOAD instruction otherwise, a STORE instruction. LOAD (0000), STORE (0001). More details are provided in the Instruction Table below.

3.	Data Instruction: Indicated by 001x Opcode. The 4th and 5th bit ARE NOT applicable here (for 0010). The 6th and 7th bits will specify register address. 0010 specifies loading a constant from a RAM to a register and 0011 specifies instruction to move values between two registers. For this instruction however, all register address bits are needed.

The value to be loaded MUST BE in the byte next to the DATA instruction. Therefore, the next instruction will be ahead by 2 bytes i.e., current instruction byte and the data byte.

4.	Jump Instruction: 
First up, is the JMPR instruction (JUMP Register Instruction) denoted by the Opcode – 0011 xx RB.
The JMP instruction jumps to address in next byte of RAM. Given by 0100 xxxx (register bits not applicable).
Then, the conditional Jump Instructions are encoded in a special format. The opcode is specified as 0101. 0101 opcode specifies that it is a conditional JUMP instruction. The next 4 bits DO NOT specify register address but status of different flags for conditional jumping. Format: 0101 CAEZ. C specifies carry output. A specifies if A is larger than B. E specifies if A = B and Z flag specifies if Answer is ZERO.

The entire condition jumping table:
C	A	E	Z	JUMP IF
0	0	0	0	No Jump
0	0	0	1	JZ
0	0	1	0	JE
0	0	1	1	JEZ
0	1	0	0	JA
0	1	0	1	JAZ
0	1	1	0	JAE
0	1	1	1	JAEZ
1	0	0	0	JC
1	0	0	1	JCZ
1	0	1	0	JCE
1	0	1	1	JCEZ
1	1	0	0	JCA
1	1	0	1	JCAZ
1	1	1	0	JCAE
1	1	1	1	JCAEZ




AVAILABLE INSTRUCTIONS:
Instruction	Opcode	Format	Description
ADD	1000	ADD RA, RB	Add contents of RA and RB. Put the answer in RB.
SHL	1001	SHL RA, RB	Shift RA to left side. Put the answer in RB.
SHR	1010	SHR RA, RB	Shift RA to right side. Put the answer in RB.
NOT	1011	NOT RA, RB	NOT RA and put answer in RB.
AND	1100	AND RA, RB	AND RA & RB and put the answer in RB.
OR	1101	OR RA, RB	OR RA & RB and put the answer in RB.
XOR	1110	XOR RA, RB	XOR RA & RB and put the answer in RB.
CMP	1111	CMP RA, RB	Compare RA and RB.
STORE	0001	ST RA, RB	Store contents of RB to RAM Address in RA.
LOAD	0000	LD RA, RB	Load RB from RAM Address in RA.
DATA	0010	DATA RB, xxxxxxxx	Load the 8 bits from the next RAM address into RB. (Load from next RAM address to REGISTER)
JMP_REGISTER	0011	JMPR RB	Jump to address RB
JMP	0100	JMP Addr	Jump to address in next byte.
JZ	01010001	JZ Addr	Jump to address in next byte IF answer is ZERO
JE	01010010	JE Addr	Jump to address in next byte IF A equals B
JA	01010100	JA Addr	Jump to address in next byte IF A is LARGER than B
JC	01011000	JC Addr	Jump to address in next byte IF CARRY is ON
JCA	01011100	JCA Addr	Jump to address in next byte IF CARRY is ON OR A is larger
JCE	01011010	JCE Addr	Jump to address in next byte IF CARRY is ON OR A is equal to B
JCZ	01011001	JCZ Addr	Jump to address in next byte IF CARRY is ON OR Answer is ZERO
JAE	01010110	JAE Addr	Jump to address in next byte IF A is larger or equal to B
JAZ	01010101	JAZ Addr	Jump to address in next byte IF A is larger than B or answer is ZERO
JEZ	01010011	JEZ Addr	Jump to address in next byte IF A equals B or Answer is ZERO
JCAE	01011110	JCAE Addr	Jump to address in next byte IF CARRY is on or A is larger or equal to B
JCAZ	01011101	JCAZ Addr	Jump to address in next byte IF CARRY is ON OR A is larger than B OR equal to ZERO
JCEZ	01011011	JCEZ Addr	Jump to address in next byte IF CARRY IS ON OR A equals B or ZERO
JAEZ	01010111	JAEZ Addr	Jump to address in next byte IF A larger than or equal to B OR ZERO.
JCAEZ	01011111	JCAEZ Addr	Jump to address in next byte IF CARRY is ON OR A is larger than B OR Equal to B OR ZERO
CLF	01100000	Clears all flags.	CLF
END	11001111	END	ENDS the program.


*** END OF INSTRUCTION SET ARCHITECTURE FILE ***

Prepared in a close collaboration with another team (21UCS134, 21UCS143).
