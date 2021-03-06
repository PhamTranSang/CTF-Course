[TutorialsPoint](https://www.tutorialspoint.com/assembly_programming/index.htm)


## Table of Contents
* [Memory Segments](#memory-segments)
* [Registers](#registers) 
* [System Calls](#system-calls)
* [Addressing](#addressing)
* [Variables](#variables)
* [Constants](#constants)
* [Instructions](#instructions)
  * [Arithmetic](#arithmetic-instructions)
  * [Logical](#logical-instruction)
* [Conditions](#conditions)


## Memory Segments

* Data segment
  * .data section: Where constants are declared
  * .bss section: Where variables are declared
* Code segment
  * .text section: Where the code is
* Stack


## Registers

### Data Registers
Complete 32-bit data registers:
* EAX
* EBX
* ECX
* EDX

Lower halves of 32-bit registers (16-bits):
* AX - Primary accumulator
  * Used in input/output and most arithmetic instructions
* BX - Base register
  * Used in indexed addressing
* CX - Count register
  * Stores loop count in iterative operations
* DX - Data register
  * Also used in input/output operations

Lower and higher halves of 16-bit registers (8-bits):
* AL, AH
* BL, BH
* CL, CH
* DL, DH

![](https://www.tutorialspoint.com/assembly_programming/images/register1.jpg "Registers")

### Pointer Registers - 16 bits
* Instruction Pointer (IP)
  * Stores the offset address of the next instruction to be executed
* Stack Pointer (SP)
  * Provides the offset value within the program stack
  * Along with SS (SS:SP), referes to current position of data/address within stack
* Base Pointer (BP)
  * Helps in referencing parameter variables passed to subroutine
  * Along with SS, refers to the location of the paramters

![](https://www.tutorialspoint.com/assembly_programming/images/register3.jpg "Registers")

### Flags Register - Many instructions will set the following flags
* Overflow flag (OF)
* Direction Flag (DF)
* Interrupt Flag (IF)
* Trap Flag (TF)
* Sign Flag (SF)
* Zero Flag (ZF)
* Auxiliary Carry Flag (AF)
* Parity Flag (PF)
* Carry Flag (CF)

## System Calls

### You need to:
* Put system call # in EAX register (system calls listed on */usr/include/asm/unistd.h*)
* Put arguments to system call in registers (EBX, ECX, EDX, ESI, EDI, EBP)
* Call the relevant interrupt (int 0x80)
* Result (usually) returned in EAX register.

### Example: sys_exit
```assembly
mov	eax,1		; system call number (sys_exit)
int	0x80		; call kernel
```

### Example: sys_write
```assembly
mov	edx,4		; message length
mov	ecx,msg		; message to write
mov	ebx,1		; file descriptor (stdout)
mov	eax,4		; system call number (sys_write)
int	0x80		; call kernel
```


## Addressing

### Register Addressing
```assembly
MOV DX, TAX_RATE   ; Register in first operand
MOV COUNT, CX	   ; Register in second operand
MOV EAX, EBX	   ; Both the operands are in registers
```

### Immediate Addressing
```assembly
BYTE_VALUE  DB  150    ; A byte value is defined
WORD_VALUE  DW  300    ; A word value is defined
ADD  BYTE_VALUE, 65    ; An immediate operand 65 is added
MOV  AX, 45H           ; Immediate constant 45H is transferred to AX
```

### Direct Memory Addressing
```assembly
ADD	BYTE_VALUE, DL	; Adds the register in the memory location
MOV	BX, WORD_VALUE	; Operand from the memory is added to register
'''

### Direct-Offset Addressing
```assembly
BYTE_TABLE DB  14, 15, 22, 45      ; Tables of bytes
WORD_TABLE DW  134, 345, 564, 123  ; Tables of words

MOV CL, BYTE_TABLE[2]	; Gets the 3rd element of the BYTE_TABLE
MOV CL, BYTE_TABLE + 2	; Gets the 3rd element of the BYTE_TABLE
MOV CX, WORD_TABLE[3]	; Gets the 4th element of the WORD_TABLE
MOV CX, WORD_TABLE + 3	; Gets the 4th element of the WORD_TABLE
```

### Indirect Memory Addressing
```assembly
MY_TABLE TIMES 10 DW 0  ; Allocates 10 words (2 bytes) each initialized to 0
MOV EBX, [MY_TABLE]     ; Effective Address of MY_TABLE in EBX
MOV [EBX], 110          ; MY_TABLE[0] = 110
ADD EBX, 2              ; EBX = EBX +2
MOV [EBX], 123          ; MY_TABLE[1] = 123
```

### MOV
```assembly
MOV  destination, source

MOV  register, register
MOV  register, immediate
MOV  memory, immediate
MOV  register, memory
MOV  memory, register
```


## Variables

### Initialized Data Syntax
```assembly
[variable-name]    define-directive    initial-value   [,initial-value]...
```

### Define Directives
| Directive     | Purpose           | Space    |
| ------------- | ----------------- | -------- |
| DB            | Define Byte       | 1 Byte   |
| DW            | Define Word       | 2 Bytes  |
| DD            | Define Doubleword | 4 Bytes  |
| DQ            | Define Quadword   | 8 Bytes  |
| DT            | Define Ten Bytes  | 10 Bytes |

### Examples
```assembly
choice		DB	'y'
number		DW	12345
neg_number	DW	-12345
big_number	DQ	123456789
real_number1	DD	1.234
real_number2	DQ	123.456
```


## Constants
```assembly
CONSTANT_NAME EQU expression
%assign CONSTANT_NAME expression ;for numeric contatns, allows redefinition
%define CONTSTANT_NAME expression ;for numeric and string constants

; examples
TOTAL_STUDENTS equ 50
%assign TOTAL 10
%assign TOTAL 20
%define PTR [EBP+4]
```


## Instructions
### Arithmetic Instructions
```assembly
INC dest     ; Increments dest by one, dest can be 8-bit, 16-bit, or 32-bit
; dest can be a register or in memory

INC EBX	     ; Increments 32-bit register
INC DL       ; Increments 8-bit register
INC [count]  ; Increments the count variable

DEC dest     ; Increments dest by one, dest can be 8-bit, 16-bit, or 32-bit
```

```assembly
ADD/SUB	destination, source	; Add or subtract 8-bit, 16-bit, 32-bit operands
```

Add/Sub can take place between:
* Register to register
* Memory to register
* Register to memory
* Register to constant data
* Memory to constant data

```assembly
MUL/IMUL multiplier
```

```assembly
DIV/IDIV divisor
```


### Logical Instructions
| Instruction       | Format                     |
| ----------------- | -------------------------- |
| AND               | AND operand1, operand2     |
| OR                | OR operand1, operand2      |
| XOR               | XOR operand1, operand2     |
| TEST              | TEST operand1, operand2    |
| NOT               | NOT operand1               |


## Conditions

### Unconditional Jump
``` assembly
JMP	label

;example
MOV  AX, 00    ; Initializing AX to 0
MOV  BX, 00    ; Initializing BX to 0
MOV  CX, 01    ; Initializing CX to 1
L20:
ADD  AX, 01    ; Increment AX
ADD  BX, AX    ; Add AX to BX
SHL  CX, 1     ; shift left CX, this in turn doubles the CX value
JMP  L20       ; repeats the statements
```

### CMP
```assembly
CMP destination, source

;example
INC	EDX
CMP	EDX, 10	; Compares whether the counter hass reached 10
JLE	LP1     ; If it is less than or equal to 10, then jump to LP1
```

### Conditional Jump
#### Signed Data for arithmetic operations
| Instruction     | Description                         | Flags       |
| --------------- | ----------------------------------- | ----------- |
| JE/JZ           | Jump Equal or Jump Zero             | ZF          |
| JNE/JNZ         | Jump Not Equal or Jump Not Zero     | ZF          |
| JG/JNLE         | Jump Greater or Jump Not Less/Equal | OF, SF, ZF  |
| JGE/JNL         | Jump Greater/Equal or Jump Not Less | OF, SF      |
| JL/JNGE         | Jump Less or Jump Not Greater/Equal | OF, SF      |
| JLE/JNG         | Jump Less/Equal or Jump Not Greater | OF, SF, ZF  |

#### Unsigned Data for logical operations
| Instruction     | Description                         | Flags       |
| --------------- | ----------------------------------- | ----------- |
| JE/JZ           | Jump Equal or Jump Zero             | ZF          |
| JNE/JNZ         | Jump Not Equal or Jump Not Zero     | ZF          |
| JA/JNBE         | Jump Above or Jump Not Below/Equal  | CF, ZF      |
| JAE/JNB         | Jump Above/Equal or Jump Not Below  | CF          |
| JB/JNAE         | Jump Below or Jump Not Above/Equal  | CF          |
| JBE/JNA         | Jump Below/Equal or Jump Not Above  | AF, CF      |


