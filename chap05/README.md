# Chap05. prinf()

## 5.1 : x86 - argument 3

```c
#include <stdio.h>

int main()
{
  printf("a=%d; b=%d; c=%d", 1, 2, 3);
  return 0;
}
```

### GCC 32bit

```
gef> disas main
Dump of assembler code for function main:
   0x0804841d <+0>:     push   ebp
   0x0804841e <+1>:     mov    ebp,esp
=> 0x08048420 <+3>:     and    esp,0xfffffff0
   0x08048423 <+6>:     sub    esp,0x10
   0x08048426 <+9>:     mov    DWORD PTR [esp+0xc],0x3
   0x0804842e <+17>:    mov    DWORD PTR [esp+0x8],0x2
   0x08048436 <+25>:    mov    DWORD PTR [esp+0x4],0x1
   0x0804843e <+33>:    mov    DWORD PTR [esp],0x80484f0
   0x08048445 <+40>:    call   0x80482f0 <printf@plt>
   0x0804844a <+45>:    mov    eax,0x0
   0x0804844f <+50>:    leave  
   0x08048450 <+51>:    ret    
```


#### GDB debugging

```
gef> 
0x08048445 in main ()
------------------------------------------------------------------------------------------------------------------------------------------[registers]--$eax     0x00000001 $ebx     0xf7fbc000 $ecx     0xf16edc9e $edx     0xffffc984 $esp     0xffffc940 $ebp     0xffffc958 
$esi     0x00000000 $edi     0x00000000 $eip     0x08048445 $cs      0x00000023 $ss      0x0000002b $ds      0x0000002b 
$es      0x0000002b $fs      0x00000000 $gs      0x00000063 $eflags  [ SF IF ] 
Flags: [ carry  parity  adjust  zero  SIGN  trap  INTERRUPT  direction  overflow  resume  virtualx86  identification ]
----------------------------------------------------------------------------------------------------------------------------------------------[stack]--0xffffc940|+0x0000: 0x080484f0 -> "a=%d; b=%d; c=%d"            <-$esp   
0xffffc944|+0x0004: 0x1
0xffffc948|+0x0008: 0x2
0xffffc94c|+0x000c: 0x3
0xffffc950|+0x0010: 0x08048460 -> <__libc_csu_init>: push ebp
0xffffc954|+0x0014: 0x0
0xffffc958|+0x0018: 0x0         <-$ebp   
0xffffc95c|+0x001c: 0xf7e2cad3 -> <__libc_start_main+243>: mov DWORD PTR [esp],eax
------------------------------------------------------------------------------------------------------------------------------------------[code:i386]--0x8048426        <main+9>     mov   DWORD PTR [esp+0xc],0x3
0x804842e        <main+17>     mov   DWORD PTR [esp+0x8],0x2
0x8048436        <main+25>     mov   DWORD PTR [esp+0x4],0x1
0x804843e        <main+33>     mov   DWORD PTR [esp],0x80484f0
0x8048445        <main+40>     call   0x80482f0 <printf@plt>             <- $pc
0x804844a        <main+45>     mov   eax,0x0
0x804844f        <main+50>     leave   
0x8048450        <main+51>     ret   
0x8048451             xchg   ax,ax
----------------------------------------------------------------------------------------------------------------------------------------------[trace]--
#0  0x08048445 in main ()
-------------------------------------------------------------------------------------------------------------------------------------------------------
gef> 
```

## 5.2 : x64 : argument 8

함수의 argument 가 많은 경우 (8개)

```c
#include <stdio.h>

int main()
{
  printf("a=%d; b=%d; c=%d, d=%d, e=%d; f=%d; g=%d; h=%d\n", 1, 2, 3, 4, 5, 6, 7, 8);
  return 0;
}
```


### GCC x32

GCC 32bit 는 이전과 같이 모두 stack 을 통하여 argument passing

```
gef> disas main
Dump of assembler code for function main:
   0x0804841d <+0>:     push   ebp
   0x0804841e <+1>:     mov    ebp,esp
   0x08048420 <+3>:     and    esp,0xfffffff0
   0x08048423 <+6>:     sub    esp,0x30
   0x08048426 <+9>:     mov    DWORD PTR [esp+0x20],0x8
   0x0804842e <+17>:    mov    DWORD PTR [esp+0x1c],0x7
   0x08048436 <+25>:    mov    DWORD PTR [esp+0x18],0x6
   0x0804843e <+33>:    mov    DWORD PTR [esp+0x14],0x5
   0x08048446 <+41>:    mov    DWORD PTR [esp+0x10],0x4
   0x0804844e <+49>:    mov    DWORD PTR [esp+0xc],0x3
   0x08048456 <+57>:    mov    DWORD PTR [esp+0x8],0x2
   0x0804845e <+65>:    mov    DWORD PTR [esp+0x4],0x1
   0x08048466 <+73>:    mov    DWORD PTR [esp],0x8048510
   0x0804846d <+80>:    call   0x80482f0 <printf@plt>
   0x08048472 <+85>:    mov    eax,0x0
   0x08048477 <+90>:    leave  
   0x08048478 <+91>:    ret    
End of assembler dump.
```


### GCC x64

32bit 에서는 모든 parameter 를 stack 을 통하여 전달했던 것과는 달리

- 처음 6개 인자는 register 를 통하여 전달 : rdi, rsi, rdx, rcx, r8, r9
- 나머지는 stack 을 통하여 전달

```
gef> disas main
Dump of assembler code for function main:
   0x000000000040052d <+0>:     push   rbp
   0x000000000040052e <+1>:     mov    rbp,rsp
   0x0000000000400531 <+4>:     sub    rsp,0x20
   0x0000000000400535 <+8>:     mov    DWORD PTR [rsp+0x10],0x8
   0x000000000040053d <+16>:    mov    DWORD PTR [rsp+0x8],0x7
   0x0000000000400545 <+24>:    mov    DWORD PTR [rsp],0x6
   0x000000000040054c <+31>:    mov    r9d,0x5
   0x0000000000400552 <+37>:    mov    r8d,0x4
   0x0000000000400558 <+43>:    mov    ecx,0x3
   0x000000000040055d <+48>:    mov    edx,0x2
   0x0000000000400562 <+53>:    mov    esi,0x1
   0x0000000000400567 <+58>:    mov    edi,0x400608
   0x000000000040056c <+63>:    mov    eax,0x0
   0x0000000000400571 <+68>:    call   0x400410 <printf@plt>
   0x0000000000400576 <+73>:    mov    eax,0x0
   0x000000000040057b <+78>:    leave  
   0x000000000040057c <+79>:    ret    
End of assembler dump.
```


## 5.3 : ARM argument 3

```
gef➤  disas main
Dump of assembler code for function main:
   0x00008444 <+0>:     push    {r11, lr}
   0x00008448 <+4>:     add     r11, sp, #4
   0x0000844c <+8>:     ldr     r0, [pc, #24]   ; 0x846c <main+40>
   0x00008450 <+12>:    mov     r1, #1
   0x00008454 <+16>:    mov     r2, #2
   0x00008458 <+20>:    mov     r3, #3
   0x0000845c <+24>:    bl      0x82e0
   0x00008460 <+28>:    mov     r3, #0
   0x00008464 <+32>:    mov     r0, r3
   0x00008468 <+36>:    pop     {r11, pc}
   0x0000846c <+40>:    andeq   r8, r0, r4, ror #9
End of assembler dump.
```




