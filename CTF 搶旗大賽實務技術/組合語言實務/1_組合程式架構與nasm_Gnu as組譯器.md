# 組譯器
```
Gnu as ==>  gas
nasm 
```

# C helloWorld
```
#include <stdio.h>

int main(){
  printf("Hello World!");
  return 0;
}
```
# 1. Gnu as
## 32bit Helloworld
```
EAX register ==> 存 系統呼叫
EBX register ==> 存 file descriptor
```
```
.global	_start

.text
_start:
	movl  $4, %eax   # 4 (code for "write" syscall) -> EAX register
	movl  $1, %ebx   # 1 (file descriptor for stdout) -> EBX (1st argument to syscall)
	movl  $msg, %ecx # address of msg string -> ECX (2nd argument)
	movl  $len, %edx # len (32 bit address) -> EDX (3rd arg)
	int   $0x80      # interrupt with location 0x80 (128), which invokes the kernel's system call procedure

	movl  $1, %eax   # 1 ("exit") -> EAX
	movl  $0, %ebx   # 0 (with success) -> EBX
	int   $0x80      # interrupt with location 0x80 (128), which invokes the kernel's system call procedure
.data
msg:
	.ascii  "Hello, world!\n" # inline ascii string
	len =   . - msg           # assign value of (current address - address of msg start) to symbol "len"
```
```
as -c helloworld.s
ld helloworld.o -o helloworld
./helloworld
```
## 64 bit helloworld.s
```
https://gist.github.com/carloscarcamo/6833d19b726af698e62b
```
```
.global _start

.text

_start:
    # write (1, msj, 13)
    mov $1, %rax            # system call 1 is write
    mov $1, %rdi            # file handler 1 is stdout
    mov $message, %rsi      # address of string to output
    mov $13, %rdx           # number of bytes
    syscall  # 64位元是用syscall 系統呼叫

    # exit(0)
    mov $60, %rax           # system call 60 is exit
    xor %rdi, %rdi          # we want to return code 0
    syscall  # 64位元是用syscall 系統呼叫

message:
    .ascii "Hello, world\n"
```
### 組譯成64位元執行檔並執行
```
gcc -c hello.s && ld hello.o -o hello2 && ./hello2

file hello2 ==>
hello2: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```
# 2.nasm程式碼架構
```
http://www.posix.nl/linuxassembly/nasmdochtml/nasmdoca.html
http://athena.nitc.ac.in/piyush_b130057cs/NASM/NASM%20TUTORIAL.pdf
```
```
由不同section組成,各section有不同功能
```

```
section .data  ==>  Section containing initialised data《有初值的資料》

section .bss   ==>  Section containing uninitialized data《沒有初值的資料》 ;可有可無的選項

section .text  ==>  ; Section containing code 《執行程式碼》
```
## 32-bit helloworld32.asm
```
;nasm 2.11.08

section .data
    hello:     db 'Hello world!',10    ; 'Hello world!' plus a linefeed character
    helloLen:  equ $-hello             ; Length of the 'Hello world!' string

section .text
	global _start

_start:
	mov eax,4            ; The system call for write (sys_write)  32位元Linux系統呼叫
	mov ebx,1            ; File descriptor 1 - standard output
	mov ecx,hello        ; Put the offset of hello in ecx
	mov edx,helloLen     ; helloLen is a constant, so we don't need to say
	                     ;  mov edx,[helloLen] to get it's actual value
	int 80h              ; Call the kernel

	mov eax,1            ; The system call for exit (sys_exit)   32位元Linux系統呼叫
	mov ebx,0            ; Exit with return code of 0 (no error)
	int 80h;
```
### 執行
```
$ nasm -f elf32 helloworld32.asm
$ ld helloworld32.o -o helloworld32
$ ./helloworld32
```
### 用hexdump看看
```
hd helloworld32.o
```
```
00000000  7f 45 4c 46 01 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  01 00 03 00 01 00 00 00  00 00 00 00 00 00 00 00  |................|
00000020  40 00 00 00 00 00 00 00  34 00 00 00 00 00 28 00  |@.......4.....(.|
00000030  07 00 03 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000a0  70 01 00 00 22 00 00 00  00 00 00 00 00 00 00 00  |p..."...........|
000000b0  10 00 00 00 00 00 00 00  0d 00 00 00 03 00 00 00  |................|
000000c0  00 00 00 00 00 00 00 00  a0 01 00 00 31 00 00 00  |............1...|
000000d0  00 00 00 00 00 00 00 00  01 00 00 00 00 00 00 00  |................|
000000e0  17 00 00 00 02 00 00 00  00 00 00 00 00 00 00 00  |................|
000000f0  e0 01 00 00 70 00 00 00  05 00 00 00 06 00 00 00  |....p...........|
00000100  04 00 00 00 10 00 00 00  1f 00 00 00 03 00 00 00  |................|
00000110  00 00 00 00 00 00 00 00  50 02 00 00 26 00 00 00  |........P...&...|
00000120  00 00 00 00 00 00 00 00  01 00 00 00 00 00 00 00  |................|
00000150  04 00 00 00 08 00 00 00  00 00 00 00 00 00 00 00  |................|
00000160  48 65 6c 6c 6f 20 77 6f  72 6c 64 21 0a 00 00 00  |Hello world!....|
00000170  b8 04 00 00 00 bb 01 00  00 00 b9 00 00 00 00 ba  |................|
00000180  0d 00 00 00 cd 80 b8 01  00 00 00 bb 00 00 00 00  |................|
00000190  cd 80 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000001a0  00 2e 64 61 74 61 00 2e  74 65 78 74 00 2e 73 68  |..data..text..sh|
000001b0  73 74 72 74 61 62 00 2e  73 79 6d 74 61 62 00 2e  |strtab..symtab..|
000001c0  73 74 72 74 61 62 00 2e  72 65 6c 2e 74 65 78 74  |strtab..rel.text|
000001d0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000240  1f 00 00 00 00 00 00 00  00 00 00 00 10 00 02 00  |................|
00000250  00 68 65 6c 6c 6f 77 6f  72 6c 64 2e 61 73 6d 00  |.helloworld.asm.|
00000260  68 65 6c 6c 6f 00 68 65  6c 6c 6f 4c 65 6e 00 5f  |hello.helloLen._|
00000270  73 74 61 72 74 00 00 00  00 00 00 00 00 00 00 00  |start...........|
00000280  0b 00 00 00 01 02 00 00  00 00 00 00 00 00 00 00  |................|
00000290
```
### 32位元Linux系統呼叫
```
http://syscalls.kernelgrok.com/
```
# 64-bit helloworld64.asm
```
; 64-bit "Hello World!" in Linux NASM

global _start            ; global entry point export for ld

section .text
_start:

    ; sys_write(stdout, message, length)  系統呼叫
    mov    rax, 1        ; sys_write
    mov    rdi, 1        ; stdout
    mov    rsi, message    ; message address
    mov    rdx, length    ; message string length
    syscall

    ; sys_exit(return_code) 系統呼叫
    mov    rax, 60        ; sys_exit
    mov    rdi, 0        ; return 0 (success)
    syscall

section .data
    message: db 'Hello, world!',0x0A    ; message and newline
    length:    equ    $-message        ; NASM definition pseudo-instruction
```
### 執行
```
$ nasm -f elf64 helloworld64.asm
$ ld helloworld64.o -o helloworld64
$ ./helloworld64
```
### 用hexdump看看
```
hd helloworld64.o
```
### 64位元Linux系統呼叫
```
http://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/

最新版的定義
https://github.com/torvalds/linux/blob/master/arch/x86/entry/syscalls/syscall_64.tbl
```

# 64-bit 作業系統開發32-bit與 64-bit 執行檔[開發環境 64-bit Kali linux]
## 範例程式 hello.asm
```
section .text
 global _start
  _start:
 mov edx,len
 mov ecx,msg
 mov ebx,1
 mov eax,4
 int 0x80
 
 mov eax,1
 int 0x80

  section .data
  msg db 'hello World!' , 0xa
  len equ $ -msg
```
## 組譯與執行 ==> 64 位元執行檔(binary)
```
nasm -f elf64 hello.asm     # assemble the program  64位元

ld hello.o -o hello

./hello
```
### binary 簡單分析
```
file hello

hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```
```
strings hello


hello World!
hello.asm
__bss_start
_edata
_end
.symtab
.strtab
.shstrtab
.note.gnu.property
.text
.data
```

## 組譯與執行 ==> 32 位元執行檔(binary)
```
nasm -f elf32 hello.asm -o hello2.o

ld -m elf_i386  hello2.o -o hello2

./hello2
```

### binary 簡單分析
```
file hello2

hello: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, not stripped
```
```
strings hello2


hello World!
hello.asm
__bss_start
_edata
_end
.symtab
.strtab
.shstrtab
.note.gnu.property
.text
.data
```

```
hexdump hello2


0000000 457f 464c 0101 0001 0000 0000 0000 0000
0000010 0002 0003 0001 0000 9000 0804 0034 0000
0000020 2128 0000 0000 0000 0034 0020 0004 0028
0000030 0007 0006 0001 0000 0000 0000 8000 0804
0000040 8000 0804 00d0 0000 00d0 0000 0004 0000
0000050 1000 0000 0001 0000 1000 0000 9000 0804
0000060 9000 0804 001d 0000 001d 0000 0005 0000
0000070 1000 0000 0001 0000 2000 0000 a000 0804

```
