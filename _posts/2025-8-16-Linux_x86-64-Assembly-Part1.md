---
layout: post
title: Introduction to x86_64 linux assembly part 1 (nasm) (GNU/linux)
---

# Introduction 
Have you ever wondered how compiled programming languages actually work behind the scenes? When you hear the word "compile" what does that mean to you?

We usually take for granted situations like this, but in this blog I'll show you how to write your own little Hello world in assembly! I will also show you the basic structure of the Executable and Linkable(ELF) format in GNU/Linux and why it is really cool (and potentially *aura farming*) to be able to write, and read assembly code!

# Prerequisites
So before we start, I'd assume you have basic knowledge of Command Line Interface(CLI) tools since we will be using the following commands:
1. [nasm (Netwide assmebler)](https://www.nasm.us/)
2. common commands such as ld (linker), mkdir, touch
3. knowledge of shells such as `zsh`, `fish`, `bash` or `sh`
4. gcc (For C programming language comparison)
5. a linux x86_64 machine (yes, assembly is very "machine" and "OS" specific, you can just assume this is intel + linux)

For linux users, I'd assume the linker `ld` would be built-in already. If not, install it through your package manager.
I personally use Arch linux, so I use the `pacman` command to install this:
```sh
sudo pacman -S nasm
```

For debian based users (such as Ubuntu, linux mint etc.)
```sh
sudo apt install nasm
```

With this out of the way, let's start with our first basic "Hello world" program!

# Your First x86_64 assembly executable
Aight, here I will start by making a new file called "hello.asm" using the command:
```sh
touch hello.asm
```
in my designated directory, then using a text editor, type this code into the file:
```nasm
BITS 64
CPU X64

global _start

SECTION .text

_start:
    ret
```

Let's analyze this code line by line.
`BITS` and `CPU` are two nasm specific instructions to tell nasm what and how to compile the assembly into, we tell it that we are using 64-bit and our CPU is of architecture x64, this will "hint" the Netwide assembler to do the right thing.
`global` simply tells nasm what label to jump to as a starting point (entry point) of the program, you can see labels are simply a line with a single name proceeded with a colon ":", everything below a label will be taken as assembly instructions, or raw bytes, similar to defining a function! Here we defined `_start` as our entry point.
`SECTION` tells nasm what part of the executable file is everything below, here we use the `.text` section, in the ELF file specification, the `.text` is used to store executable and readable assembly code!
`_start:` is just a label that our assembly code can refer to, here it's referring to the instructions below.
And finally, `ret` simply tells the program to return and exit!

Now, after saving the file, we run the following commands to assemble the file into an object file for Executable and [Linkable File format](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format), which in layman's terms, this is the executable format for linux.

We proceed with the following command:
```sh
nasm -f ELF64 hello.asm
```
This simply assembles the code into an intermediate object file called "hello.o" so we can start linking into the final executable ELF file, think of it like preparing libraries, or preparing sauce before the final bread in a sandwich :P. The intermediate object file contain machine code, symbols, sections, identifiers and many more so other linkers can link multiple object files. Well I'm kind of getting ahead are we? Lets continue.

```sh
ld hello.o -o hello
```
As you can probably guess, `ld` is the gnu linker, it simply links multiple object files together just like gluing multiple parts of the final machine! Here we only have one file to work with, so we simply just link `hello.o` and output into the ELF executable file called `hello` 

Voila~ You have now written your first assembly program! If nothing goes wrong, typing the following command:
```sh
file hello
```

should show you the properties of a correct ELF64 binary, here is what mine shows:
```sh
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```

to run the program, simply do `./hello`!

# The x86_64 Assembly
So if you have finished compiling and running your first assembly program, you will likely realize it... uhh didn't output any "Hello World" and even thrown an error similar to the message below:
```
fish: Job 1, './main' terminated by signal SIGSEGV (Address boundary error)
```

I am using the `fish` shell, depending on the environment(shell and implementation of assembler / linker) you are using, it might output different messages, but overall it should give you a SEGMENTATION FAULT error (SIGSEGV)

Before we continue on, I should explain the basics of Registers, syscalls and syntax of x86_64 assembly.

## syntax
In x86_64 assembly, instructions are usually written like this:
```
<abbreviated instruction> <arg1>, <arg2>, <arg3> ...
```

Usually assembly instructions are atomic, meaning that each instruction does one simple thing and nothing more.

here are some examples:
```
mov rax, 1
```

`mov` is an instruction of loading a value into a register. I will explain further on what registers are later! But you can understand it as an assignment to a variable, just like `rax = 1`!

## Registers
If you have at least learnt the basics of programming, you would have known variables are simply storage for data with a given name!
Registers on the other hand have a similar concept, though the names are not within your control and the types are fixed as well. A register can store from 8 bits, up to 512 bits on a 64-bit machine!

The usual naming convention of a register is as follows: `<prefix><abbreviation / name>`

commonly in x86_64, you would have these 64-bit registers:
`rax`, `rbx`, `rcx`, `rdx`, `rdi`, `rsi`, `rbp`, `rsp`, `r8`, `r9`, `r10`

Just like the example we have given in the syntax section, you can see that, `mov rax, 1` simply loads / stores 1 into the register `rax` which is 64-bit.

A register can have multiple tinier counterparts, we use `rax` register for an example, it's counterpart is `eax`, which is halved of the previous bits (32-bit) instead!
Same goes with `rbx`, `rcx`, `rdx`, `rdi`, `rsi` and `rsp`! Each could be replaced from `r` to an `e` to grab it's 32-bit counterpart.

Now the issue is this, whenever you change certain values in `rax`, it seems `eax` and it's counterparts also change! Why does this happen?

Here's a ![picture](https://imgur.com/a/scTwgn5) I have beautifully drawn to illustrate this point:

![Registers](https://imgur.com/a/scTwgn5)

As you can see above, the counterpart 32-bit register is actually situated within `rax` register itself! If you look closely, it's also specifically situated in the [least significant byte](https://en.wikipedia.org/wiki/Bit_numbering), which means that this byte when changed, modifies entire value the least.

The `e` counterparts are also not the most smallest counterparts of other registers! In fact, there's one 16-bit(`ax`) and two 8-bit(`ah` and `al`) counterparts for `eax`!

You can see in detail within this [stack overflow answer](https://reverseengineering.stackexchange.com/questions/19693/how-many-registers-does-an-x86-64-cpu-actually-have)!

Now onto syscalls!

## system calls
In assembly, especially in linux, you have the concept of `syscalls`, they are simply instructions to tell your system what to do. Similar to functions in programming languages, they also take arguments!
In your normal programming language, you would probably have similar prebuilt functions such as `print`, or `writeln`. They all internally utilize `syscalls`(with a bit more work)!

The `syscalls` we are using today are simple, `write`, `read` and `exit`. On linux, each `syscall` has their own unique number(abbreviated as "NR"). Here `write`'s syscall NR is 1, `read`'s syscall NR is 0 and `exit`'s syscall NR is 60.

Every `syscall`, just like functions in a programming language, also have their own arguments! But now maybe you will ask: "How do you pass arguments to syscalls?", well registers of course! So you see, in assembly, we have something called a [call convention](https://en.wikipedia.org/wiki/X86_calling_conventions), it simply is a... well convention on how to give arguments to commands like syscalls! Here we are using the x86_64 architecture, hence it will be x86_64 call conventions.
In short, when we call a syscall, we should load all of our arguments into the respective registers. In x86 call convention, the argument order is: `rdi`, `rsi`, `rdx`, `r10`, `r8`, and `r9`. Our first argument is `rdi`, second is `rsi` and so on so forth.

The `syscalls` are call by the instruction called: `syscall`. Very self explanatory! But you see, the `syscall` instruction itself does not take any arguments! It expects `rax` to contain the syscall NR to call to. So for example, to call the `write` syscall, we have to `mov rax, 1` (because the syscall NR of `write` is 1) and then we proceed with `syscall`!

According to this table from [chromium-os' linux systemcall table](https://www.chromium.org/chromium-os/developer-library/reference/linux-constants/syscalls/) we can see that:
NR | syscall name | rax  | rdi             | rsi               | rdx          |
0  | read         | 0x0  | unsigned int fd | char *buf         | size_t count |
1  | write        | 0x1  | unsigned int fd | const char *buf   | size_t count |
60 | exit         | 0x3c | int error_code  | -                 | -            |

Here you can see, in `rax` we are storing hexadecimal version of their system call NR, 0x0 = 0, 0x1 = 1, 0x3c = 60. On the right side of the table, it also explains what arguments correspond with which register. Pretty cool right! I will explain further in the next blog for this, for now let's just focus on using `exit`!

Phew, that's a lot of words and definitions! Now you understand more on what assembly is like, we can start writing our first ever code!

## Your first NON crashing x86_64 assembly executable
Replace our assembly code with the following:

```nasm
BITS 64
CPU X64

global _start

SECTION .text

_start:
    mov rax, 60
    mov rdi, 0
    syscall
```

The Compile and Link:
```
nasm -f elf64 hello.asm
ld hello.o -o hello
```

Then we run it with `./hello` and we should see no errors and nothing has displayed! That's one step further to a simple hello world application!
As you can see, we have replaced the previous return(`ret`) instruction into two `mov` and `syscall` instruction!
`mov rax, 60` means we want to tell syscall instruction to call the `exit` syscall. Just like in C or C++, at the end of the program, we have to return a value as the [exit code](https://en.wikipedia.org/wiki/Exit_status), and a the default normal exit code should be 0.
Where do we then put this 0? Simple, according to the syscall call convention, `exit` syscall takes in the exit code from the first argument, which is `rdi` in this case, hence the line `mov rdi, 0`.

Voila~ we can finally call syscall at the end!

If you run this program, you will see nothing, on linux, you can use the `$status` environment variable to indicate the previous command's exit status:
```sh
./hello && echo exit status: $status
```

you should see:
```
exit status: 0
```

Yay! That's a lot of stuff no? This is the end of part 1, in the next blog, I will teach you how to store strings in the `.data` section, and how to read assembly code! Stay tuned!

Until then, Cheers :P
