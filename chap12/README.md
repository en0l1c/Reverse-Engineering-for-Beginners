# Chap12. switch()/case/default


```c
#include <stdio.h>

void f(int a)
{
  switch(a)
  {
    case 0: printf("zero\n"); break;
    case 1: printf("one\n"); break;
    case 2: printf("two\n"); break;
    default: printf("something unknown\n"); break;
  }
}

int main()
{
  f(2);
}
```


## GCC 32bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x0804846d <+0>:     push   ebp
   0x0804846e <+1>:     mov    ebp,esp
   0x08048470 <+3>:     and    esp,0xfffffff0
   0x08048473 <+6>:     sub    esp,0x10
   0x08048476 <+9>:     mov    DWORD PTR [esp],0x2
   0x0804847d <+16>:    call   0x804841d <f>
   0x08048482 <+21>:    leave  
   0x08048483 <+22>:    ret    
End of assembler dump.
```

```
gef➤  disas f
Dump of assembler code for function f:
   0x0804841d <+0>:     push   ebp
   0x0804841e <+1>:     mov    ebp,esp
   0x08048420 <+3>:     sub    esp,0x18
   0x08048423 <+6>:     mov    eax,DWORD PTR [ebp+0x8]
   0x08048426 <+9>:     cmp    eax,0x1
   0x08048429 <+12>:    je     0x8048442 <f+37>
   0x0804842b <+14>:    cmp    eax,0x2
   0x0804842e <+17>:    je     0x8048450 <f+51>
   0x08048430 <+19>:    test   eax,eax
   0x08048432 <+21>:    jne    0x804845e <f+65>
   0x08048434 <+23>:    mov    DWORD PTR [esp],0x8048520
   0x0804843b <+30>:    call   0x80482f0 <puts@plt>
   0x08048440 <+35>:    jmp    0x804846b <f+78>
   0x08048442 <+37>:    mov    DWORD PTR [esp],0x8048525
   0x08048449 <+44>:    call   0x80482f0 <puts@plt>
   0x0804844e <+49>:    jmp    0x804846b <f+78>
   0x08048450 <+51>:    mov    DWORD PTR [esp],0x8048529
   0x08048457 <+58>:    call   0x80482f0 <puts@plt>
   0x0804845c <+63>:    jmp    0x804846b <f+78>
   0x0804845e <+65>:    mov    DWORD PTR [esp],0x804852d
   0x08048465 <+72>:    call   0x80482f0 <puts@plt>
   0x0804846a <+77>:    nop
   0x0804846b <+78>:    leave  
   0x0804846c <+79>:    ret    
```


## GCC 64bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x000000000040057a <+0>:     push   rbp
   0x000000000040057b <+1>:     mov    rbp,rsp
   0x000000000040057e <+4>:     mov    edi,0x2
   0x0000000000400583 <+9>:     call   0x40052d <f>
   0x0000000000400588 <+14>:    pop    rbp
   0x0000000000400589 <+15>:    ret    
End of assembler dump.
```

```
gef➤  disas f
Dump of assembler code for function f:
   0x000000000040052d <+0>:     push   rbp
   0x000000000040052e <+1>:     mov    rbp,rsp
   0x0000000000400531 <+4>:     sub    rsp,0x10
   0x0000000000400535 <+8>:     mov    DWORD PTR [rbp-0x4],edi
   0x0000000000400538 <+11>:    mov    eax,DWORD PTR [rbp-0x4]
   0x000000000040053b <+14>:    cmp    eax,0x1
   0x000000000040053e <+17>:    je     0x400555 <f+40>
   0x0000000000400540 <+19>:    cmp    eax,0x2
   0x0000000000400543 <+22>:    je     0x400561 <f+52>
   0x0000000000400545 <+24>:    test   eax,eax
   0x0000000000400547 <+26>:    jne    0x40056d <f+64>
   0x0000000000400549 <+28>:    mov    edi,0x400614
   0x000000000040054e <+33>:    call   0x400410 <puts@plt>
   0x0000000000400553 <+38>:    jmp    0x400578 <f+75>
   0x0000000000400555 <+40>:    mov    edi,0x400619
   0x000000000040055a <+45>:    call   0x400410 <puts@plt>
   0x000000000040055f <+50>:    jmp    0x400578 <f+75>
   0x0000000000400561 <+52>:    mov    edi,0x40061d
   0x0000000000400566 <+57>:    call   0x400410 <puts@plt>
   0x000000000040056b <+62>:    jmp    0x400578 <f+75>
   0x000000000040056d <+64>:    mov    edi,0x400621
   0x0000000000400572 <+69>:    call   0x400410 <puts@plt>
   0x0000000000400577 <+74>:    nop
   0x0000000000400578 <+75>:    leave  
   0x0000000000400579 <+76>:    ret    
End of assembler dump.
```

## ARM

```
gef➤  disas main
Dump of assembler code for function main:
   0x000084b4 <+0>:     push    {r11, lr}
   0x000084b8 <+4>:     add     r11, sp, #4
   0x000084bc <+8>:     mov     r0, #2
   0x000084c0 <+12>:    bl      0x8440 <f>
   0x000084c4 <+16>:    mov     r0, r3
   0x000084c8 <+20>:    pop     {r11, pc}
End of assembler dump.
```


```
gef➤  disas f
Dump of assembler code for function f:
   0x00008440 <+0>:     push    {r11, lr}
   0x00008444 <+4>:     add     r11, sp, #4
   0x00008448 <+8>:     sub     sp, sp, #8
   0x0000844c <+12>:    str     r0, [r11, #-8]
   0x00008450 <+16>:    ldr     r3, [r11, #-8]
   0x00008454 <+20>:    cmp     r3, #1
   0x00008458 <+24>:    beq     0x8478 <f+56>
   0x0000845c <+28>:    cmp     r3, #2
   0x00008460 <+32>:    beq     0x8484 <f+68>
   0x00008464 <+36>:    cmp     r3, #0
   0x00008468 <+40>:    bne     0x8490 <f+80>
   0x0000846c <+44>:    ldr     r0, [pc, #48]   ; 0x84a4 <f+100>
   0x00008470 <+48>:    bl      0x82dc
   0x00008474 <+52>:    b       0x849c <f+92>
   0x00008478 <+56>:    ldr     r0, [pc, #40]   ; 0x84a8 <f+104>
   0x0000847c <+60>:    bl      0x82dc
   0x00008480 <+64>:    b       0x849c <f+92>
   0x00008484 <+68>:    ldr     r0, [pc, #32]   ; 0x84ac <f+108>
   0x00008488 <+72>:    bl      0x82dc
   0x0000848c <+76>:    b       0x849c <f+92>
   0x00008490 <+80>:    ldr     r0, [pc, #24]   ; 0x84b0 <f+112>
   0x00008494 <+84>:    bl      0x82dc
   0x00008498 <+88>:    nop                     ; (mov r0, r0)
   0x0000849c <+92>:    sub     sp, r11, #4
   0x000084a0 <+96>:    pop     {r11, pc}
   0x000084a4 <+100>:   andeq   r8, r0, r0, asr #10
   0x000084a8 <+104>:   andeq   r8, r0, r8, asr #10
   0x000084ac <+108>:   andeq   r8, r0, r12, asr #10
   0x000084b0 <+112>:   andeq   r8, r0, r0, asr r5
End of assembler dump.
```

