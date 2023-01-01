
# How to code "Hello world" in assembly 8086 

Before deeping into Assembly, I am going to use libc6-dev-i386, if you have not installed yet:

```bash
sudo apt install libc6-dev-i386
```

Let me explain why we need to use assembly or why it can be useful to learn ?


Assembly language is used primarily for direct hardware manipulation, access to specialized processor instructions, or to address critical performance issues. 
Typical uses are device drivers, low-level embedded systems, and real-time systems. If you ever thought about manipulating the instructions or injecting some piece of code 
such as in hardware hacking.Then, it will be useful to learn assembly, but it can be very complex and will require deep understanding of it.

## Is it possible to hack hardware with Assembly language ?

It is theoretically possible to use assembly language to hack hardware, but it is a complex and advanced task that requires a deep understanding of both the hardware and the assembly language being used. Assembly language is a low-level programming language that is used to directly control the hardware of a computer or other device. It is typically used to write operating systems, device drivers, and other programs that need to interface directly with the hardware.

## Why do we want to use ?

Assembly language is a low-level programming language that is used to directly control the hardware of a computer or other device. It is typically used to write operating systems,
device drivers, and other programs that need to interface directly with the hardware.Do not forget! It is important to note that attempting to hack hardware without a deep understanding
of the consequences can be extremely dangerous and can result in damage to the hardware or even physical injury.

Lets get started....

First of all, we will be using Linux system calls because you can make use of it into your asm code.

1. We need to put the system call number in the register (EAX)
2. We also need to store the arguments to the system in the register EBX,ECX
3. After this process, we need to call the relevant int 80h
3. This will be stored to the register EAX

We should consider that, there are six registers which store the arguments of the system call. These are the 
```
- EBX
- ECX
- EDX
- ESI
- EBP
- EDI
```
system call exit

```asm
mov eax, 1  ;System call 
int 0x80    ;call kernel
```


Write to the system call

```asm
mov eax, 4 ; message length
mov ebx, 1 ; file descriptor
lea ecx, [message] ; The lea instruction places the address specified by its operand
mov edx, 13 ; the length of the ascii string
int 0x80 ; call system

```


```asm
.global _start
.intel_syntax
.section .text

_start:
        # write syscall

        mov %eax, 4
        mov %ebx, 1
        lea %ecx, [message]
        mov %edx, 13
        int 0x80

        # exit syscall

        mov %eax, 1
        mov %ebx, 65
        int 0x80

# Write a string to stdout

.section .data
        message:
        .ascii "Hello, world\n"
        
 ```
 
The compilation process

```bash
as x86.asm --32 -o hello.o
gcc -o hello.elf -m32 hello.o -nostdlib
./hello.elf
```
 
 The output
 
 ```bash
 
trojan@virus:~/assembly$ ./hello.elf
Hello, world
 
 ```
 
 This is just an easy code. I will try to code more complex structure. You can also consider this as welcome message to my assembly repository.
 Stay tuned!!!
