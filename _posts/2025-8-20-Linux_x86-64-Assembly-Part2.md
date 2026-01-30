---
layout: post
title: Introduction to x86_64 linux assembly part 2 (nasm) (GNU/linux)
---

## Introduction
As explained in the last [blog](posts/2025/8/16/Linux_x86-64-Assembly-Part1.md) we have made our very own first x86_64 linux assembly executable! It does not do much, that's why in this part, we will build off of what we have there!

## The write syscall
As you can see, this is the assembly code we have left from the previous part:
```nasm
CPU X64

global _start

SECTION .text

_start:
    mov rax, 60
    mov rdi, 0
    syscall
```

This program simply creates a 64-bit ELF executable and runs the `_start` label in the readable and executable section `.text` which simply uses the `exit` syscall with the NR 60 and exits with the 0 exit status.


Now let's try to use our `write` syscall shall we? Here is the call convention, and it's syscall arguments:

| NR  | syscall name | rax | rdi             | rsi             | rdx          |

| 1   | write        | 0x1 | unsigned int fd | const char *buf | size_t count |

The `rax` register stores the syscall's NR, the `rdi` register stores the [file descriptors](https://en.wikipedia.org/wiki/File_descriptor), which in short, they are the standard I/O pipes, the stdin(0), stdout(1) and stderr(2). Here we will of course "write" to the stdout I/O file descriptor, so we will load the value 1 to the register `rdi`. `rsi` receives a character buffer, similar to a [C string](https://www.geeksforgeeks.org/c/strings-in-c/) it will write the contents of the buffer to the output of our choice. Finally, count refers to the amount of bytes / characters to use from the buffer. In this case, it should be the length of our message!


Now with that out of the way, let's learn how to store bytes or strings within an assembly program! Let's welcome the .data section!

## The .data section
According to the ~~very unreliable source~~ of [wikipedia](https://en.wikipedia.org/wiki/Data_segment) the .data section (a.k.a. the data segment) is:
> is a portion of an [object file](https://en.wikipedia.org/wiki/Object_file "Object file") or the corresponding [address space](https://en.wikipedia.org/wiki/Address_space "Address space") of a program that contains initialized [static variables](https://en.wikipedia.org/wiki/Static_variable "Static variable"), that is, [global variables](https://en.wikipedia.org/wiki/Global_variable "Global variable") and [static local variables](https://en.wikipedia.org/wiki/Static_local_variable "Static local variable").

In layman's terms, it's simply a section within an ELF file format (your executable file in linux) to store variables that are constant, or used everywhere! This section is `readable` and `writable` at the same time. Perfect for our literal strings.


Now let's open our assembly file `hello.asm` and change it to this:
```nasm
CPU X64

global _start

SECTION .text

_start:
    mov rax, 60
    mov rdi, 0
    syscall

SECTION .data
message: db "Hello world!"
```

As you can see we have added two new lines at the bottom, `SECTION .data`, just like `SECTION .text`, defines a section within an ELF executable file that the data below are within the `.data` section.


The line below that `message: db "Hello world!"` defines a label `message`(just like how we defined `_start` label) with the bytes (`db` stands for define bytes) of characters "Hello world!". Do note unlike C strings, we do not end the string with a NULL terminator (\0), because we know the length of the string, we can tell the `write` syscall to print exactly the amount of characters we want it to do.


We will always refer to the message utilizing the `message` label. This makes it similar to how variables work in programming languages.


Next up, let's finally use the `write` syscall with our first message! Observe the following assembly code:
```nasm
BITS 64
CPU X64

global _start

SECTION .text

_start:
    mov rax, 1
    mov rdi, 1
    lea rsi, [message]
    mov rdx, 13
    syscall

    mov rax, 60
    mov rdi, 0
    syscall

SECTION .data
message: db "Hello world!", 10
```

That's a lot of lines added, isn't it? Don't worry, I will explain one by one, shall we?


the first line I added is `mov rax, 1`, just like I have said in the previous blog, in the linux call convention, `rax` is used by the `syscall` instruction to identify which system call we wanted to utilize based on their syscall number(NR). We are using `write`, which has a syscall NR with 1. Hence we load `rax` register with 1.


The second line `mov rdi, 1`, is the first argument to `write`, if you remember the write syscall reference table above, the first argument refers to the file descriptor, we will be using stdout, which is 1 as it's file descriptor. Hence we load 1 into `rdi` register.


The third line is new! But it's inner workings is simple, `lea` stands for load effective address. Which in simpler terms, it will calculate the address of what the right side is pointing to, then assign the address itself into `rsi`, effectively loading a pointer! Here, the `message` label is like an address pointing to the `Hello world!` string. the square brackets \[\] around the label simply tells the assembler that this is supposed to be a pointer! If you know C or C++, it should look like this:
```c
// DATA SECTION
const char *message = "Hello world!\n";


// TEXT SECTION
const char *rsi_register = message; // message is a pointer, you are loading the pointer into the rsi register
```

The fourth line: `mov rdx, 13` refers to the argument `count`, which tells the `write` syscall how much characters / bytes to write to the output. We know "Hello world!\n" (including the newline) is 13 characters long. So it makes sense we load 13 into `rdx` register.


Finally, we call `syscall`, we compile and run, and bam! our VERY OWN FIRST x86_64 Hello world assembly program!
```sh
nasm -f elf64 hello.asm
ld hello.o -o hello
```

To run the program with exit code status:
```sh
./hello && echo exit status: $status
```

If you did everything correctly up until this point, you should see this as your output:
```
Hello world!
exit status: 0
```

**Congratulations**!

If you are curious about the size difference between the assembly program compared to a normal C program, you can use the command `ls -l` to view each compiled file. On my machine, the assembly program is just `8.7` kilobytes. A regular C hello world program utilizing the `stdio.h` header and the `printf` function is `16` kilobytes! Our assembly program is nearly half the size of the Hello world C program! How cool is that~

## What's next?
Now you know how to read, write, assemble, compile and link your own assembly program. It is safe to say in the next part, we will be learning how to reverse engineer and read a C program to learn assembly by dissecting the executable in the first place using `objdump`!

Stay tuned~


Until next time, Cheers!
