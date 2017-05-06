# Chap07

```c
#include <stdio.h>

int f (int a, int b, int c)
{
  return a * b + c;
}

int main()
{
  printf("%d\n", f(1,2,3));
  return 0;
}
```


## GCC 32bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x08048430 <+0>:     push   ebp
   0x08048431 <+1>:     mov    ebp,esp
   0x08048433 <+3>:     and    esp,0xfffffff0
   0x08048436 <+6>:     sub    esp,0x10
   0x08048439 <+9>:     mov    DWORD PTR [esp+0x8],0x3
   0x08048441 <+17>:    mov    DWORD PTR [esp+0x4],0x2
   0x08048449 <+25>:    mov    DWORD PTR [esp],0x1
   0x08048450 <+32>:    call   0x804841d <f>
   0x08048455 <+37>:    mov    DWORD PTR [esp+0x4],eax
   0x08048459 <+41>:    mov    DWORD PTR [esp],0x8048500
   0x08048460 <+48>:    call   0x80482f0 <printf@plt>
   0x08048465 <+53>:    mov    eax,0x0
   0x0804846a <+58>:    leave  
   0x0804846b <+59>:    ret    
End of assembler dump.
```

```
gef➤  disas f
Dump of assembler code for function f:
   0x0804841d <+0>:     push   ebp
   0x0804841e <+1>:     mov    ebp,esp
   0x08048420 <+3>:     mov    eax,DWORD PTR [ebp+0x8]
   0x08048423 <+6>:     imul   eax,DWORD PTR [ebp+0xc]
   0x08048427 <+10>:    mov    edx,eax
   0x08048429 <+12>:    mov    eax,DWORD PTR [ebp+0x10]
   0x0804842c <+15>:    add    eax,edx
   0x0804842e <+17>:    pop    ebp
   0x0804842f <+18>:    ret    
End of assembler dump.
```


## GCC 64bit

- `edi` : 1st argument
- `esi` : 2nd argument
- `edx` : 3rd argument

```
gef➤  disas main
Dump of assembler code for function main:
   0x000000000040054a <+0>:     push   rbp
   0x000000000040054b <+1>:     mov    rbp,rsp
=> 0x000000000040054e <+4>:     mov    edx,0x3
   0x0000000000400553 <+9>:     mov    esi,0x2
   0x0000000000400558 <+14>:    mov    edi,0x1
   0x000000000040055d <+19>:    call   0x40052d <f>
   0x0000000000400562 <+24>:    mov    esi,eax
   0x0000000000400564 <+26>:    mov    edi,0x400604
   0x0000000000400569 <+31>:    mov    eax,0x0
   0x000000000040056e <+36>:    call   0x400410 <printf@plt>
   0x0000000000400573 <+41>:    mov    eax,0x0
   0x0000000000400578 <+46>:    pop    rbp
   0x0000000000400579 <+47>:    ret    
End of assembler dump.
```


```
gef➤  disas f
Dump of assembler code for function f:
   0x000000000040052d <+0>:     push   rbp
   0x000000000040052e <+1>:     mov    rbp,rsp
   0x0000000000400531 <+4>:     mov    DWORD PTR [rbp-0x4],edi
   0x0000000000400534 <+7>:     mov    DWORD PTR [rbp-0x8],esi
   0x0000000000400537 <+10>:    mov    DWORD PTR [rbp-0xc],edx
   0x000000000040053a <+13>:    mov    eax,DWORD PTR [rbp-0x4]
   0x000000000040053d <+16>:    imul   eax,DWORD PTR [rbp-0x8]
   0x0000000000400541 <+20>:    mov    edx,eax
   0x0000000000400543 <+22>:    mov    eax,DWORD PTR [rbp-0xc]
   0x0000000000400546 <+25>:    add    eax,edx
   0x0000000000400548 <+27>:    pop    rbp
   0x0000000000400549 <+28>:    ret    
End of assembler dump.
```


## ARM 32bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x00008480 <+0>:     push    {r11, lr}
   0x00008484 <+4>:     add     r11, sp, #4
   0x00008488 <+8>:     mov     r0, #1
   0x0000848c <+12>:    mov     r1, #2
   0x00008490 <+16>:    mov     r2, #3
   0x00008494 <+20>:    bl      0x8444 <f>
   0x00008498 <+24>:    mov     r3, r0
   0x0000849c <+28>:    ldr     r0, [pc, #16]   ; 0x84b4 <main+52>
   0x000084a0 <+32>:    mov     r1, r3
   0x000084a4 <+36>:    bl      0x82e0
   0x000084a8 <+40>:    mov     r3, #0
   0x000084ac <+44>:    mov     r0, r3
   0x000084b0 <+48>:    pop     {r11, pc}
   0x000084b4 <+52>:    andeq   r8, r0, r12, lsr #10
End of assembler dump.
```

```
gef➤  disas f
Dump of assembler code for function f:
   0x00008444 <+0>:     push    {r11}           ; (str r11, [sp, #-4]!)
   0x00008448 <+4>:     add     r11, sp, #0
   0x0000844c <+8>:     sub     sp, sp, #20
   0x00008450 <+12>:    str     r0, [r11, #-8]
   0x00008454 <+16>:    str     r1, [r11, #-12]
   0x00008458 <+20>:    str     r2, [r11, #-16]
   0x0000845c <+24>:    ldr     r3, [r11, #-8]
   0x00008460 <+28>:    ldr     r2, [r11, #-12]
   0x00008464 <+32>:    mul     r2, r3, r2
   0x00008468 <+36>:    ldr     r3, [r11, #-16]
   0x0000846c <+40>:    add     r3, r2, r3
   0x00008470 <+44>:    mov     r0, r3
   0x00008474 <+48>:    add     sp, r11, #0
   0x00008478 <+52>:    ldmfd   sp!, {r11}
   0x0000847c <+56>:    bx      lr
End of assembler dump.
```






