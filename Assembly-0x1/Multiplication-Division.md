# Multiplication and Division Instructions

Integer multiplication in x86 assembly language can be performed as a 32,16,8 bit
operation. In many cases, it revolves around EAX or one of its subsets(AX,AL)
The MUL and IMUL instructions perform unsigned and signed integer multiplication, respectively.
The DIV instruction performs unsigned integer division, and IDIV performs signed integer

## MUL-instruction
The MUL (unsigned) instructions comes in three version:
The first version multiplies an 8-bit operand by the AL register. The second version multpiplies a 16-bit opernad by the AX register
,and the third verson multiplies a 32-bit operand by the EAX register.

```asm
Multiplicand:      Multiplier:      Product:        
		AL                  reg/mem8          AX
		AX                  reg/mem16         DX:AX
		EAX                 reg/mem32         EDX:EAX


upperhalf:        Lowerhalf:
		AH                      AL
		DX                      AX
		EDX                     EAX
		RDX                     RAX

```
## IMUL-instructions

The IMUL (signed multiply) instruction performs signed integer multiplication. Unlike the
MUL instruction, IMUL preserves the sign of the product. It does this by sign extending the
highest bit of the lower half of the product into the upper bits of the product. The x86 instruction
set supports three formats for the IMUL instruction: one operand, two operands, and three operands. In the one-operand format, the multiplier and multiplicand are the same size and the product is twice their size.
One-Operand Formats The one-operand formats store the product in AX, DX:AX, or
EDX:EAX:

```asm

IMUL reg/mem8 ; AX = AL * reg/mem8
IMUL reg/mem16 ; DX:AX = AX * reg/mem16
IMUL reg/mem32 ; EDX:EAX =  EAX * reg/mem32

```



```asm
global MAIN 

MAIN:
		mov al, 5h
		mov bl, 10h
		mul bl

		al --> 05 ;(8bit)
		bl --> 10 ;(8bit)
		
		ax ---> 0050 ; (16bit)
		cf ---> 0





section .data
val1 WORD 2000h
val2 WORD 0100h

global MAIN:

MAIN:

mov ax, val1 ; AX = 2000h
mul val2     ; DX:AX = 00200000h, cf = 1


; AX ---> 2000
; BX ---> 0100

; DX ---> 0020



global MAIN

MAIN:
		mov eax, 12345h
		mov ebx, 1000h
		mul ebx

```


```asm
section .data

word1 SWORD 4
dword1 SDWORD 4

section .code

mov ax, -16   ;ax = -16
mov bx, 2     ;bx = 2
imul bx,ax    ;bx = -32
imul bx, 2    ;bx = -64
imul bx, word1;bx = -256
mov eax, -16  ;eax = -16
mov ebx, 2    ;ebx = 2
imul ebx, eax ; ebx = -32
imul ebx,2    ; ebx = -64
imul ebx, dword1 ; ebx = -256


```


## Programming Execution Times


```asm

Programming execution times


section .data
StartTime DWORD ?
ProcTime DWORD ?
ProcTime DWORD ?


section .code

call GetMseconds ; get start time
mov eax, StartTime
call FirstproceduretoTest ; get stop time
sub eax, StartTime ; calculated the elapsed time
mov ProcTime, eax ; save the elapsed time

```


```asm

section .data

varA dq 2
varB dq 20
varC dq 10
result dq 0
divzero db 0


section .text
global CMAIN

CMAIN:
		; result = (varA * -100) / (varB - Varc)
		mov rax -100
		imul qword[varA] ; rdx:rax = varA * 100
		
		mov rbx, [varB]
		sub rbx, [varC] ; rbx = varB - varC

		jz divByZero ; jump if zero
		idiv rbx
		mov [result], rax
		mov byte[divzero], 0
		jmp end

divByZero:
		mov byte[divzero], 1


end:
		xor rax, rax

```
