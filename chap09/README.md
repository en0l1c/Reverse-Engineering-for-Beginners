# Chap09. Pointer

```c
#include <stdio.h>

void f1 (int x, int y, int *sum, int *product)
{
  *sum = x + y;
  *product = x * y;
};

int sum, product;

void main()
{
  f1(123, 456, &sum, &product);
  printf("sum=%d, product=%d\n", sum, product);
};
```


## GCC 32bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x0804843d <+0>:     push   ebp
   0x0804843e <+1>:     mov    ebp,esp
   0x08048440 <+3>:     and    esp,0xfffffff0
   0x08048443 <+6>:     sub    esp,0x10
   0x08048446 <+9>:     mov    DWORD PTR [esp+0xc],0x804a028
   0x0804844e <+17>:    mov    DWORD PTR [esp+0x8],0x804a024
   0x08048456 <+25>:    mov    DWORD PTR [esp+0x4],0x1c8
   0x0804845e <+33>:    mov    DWORD PTR [esp],0x7b
=> 0x08048465 <+40>:    call   0x804841d <f1>
   0x0804846a <+45>:    mov    edx,DWORD PTR ds:0x804a028
   0x08048470 <+51>:    mov    eax,ds:0x804a024
   0x08048475 <+56>:    mov    DWORD PTR [esp+0x8],edx
   0x08048479 <+60>:    mov    DWORD PTR [esp+0x4],eax
   0x0804847d <+64>:    mov    DWORD PTR [esp],0x8048520
   0x08048484 <+71>:    call   0x80482f0 <printf@plt>
   0x08048489 <+76>:    leave  
   0x0804848a <+77>:    ret    
End of assembler dump.
```

```
Dump of assembler code for function f1:
   0x0804841d <+0>:     push   ebp
   0x0804841e <+1>:     mov    ebp,esp
   0x08048420 <+3>:     mov    eax,DWORD PTR [ebp+0xc]
   0x08048423 <+6>:     mov    edx,DWORD PTR [ebp+0x8]
   0x08048426 <+9>:     add    edx,eax
   0x08048428 <+11>:    mov    eax,DWORD PTR [ebp+0x10]
   0x0804842b <+14>:    mov    DWORD PTR [eax],edx
   0x0804842d <+16>:    mov    eax,DWORD PTR [ebp+0x8]
   0x08048430 <+19>:    imul   eax,DWORD PTR [ebp+0xc]
   0x08048434 <+23>:    mov    edx,eax
   0x08048436 <+25>:    mov    eax,DWORD PTR [ebp+0x14]
   0x08048439 <+28>:    mov    DWORD PTR [eax],edx
   0x0804843b <+30>:    pop    ebp
   0x0804843c <+31>:    ret    
End of assembler dump.
```

```
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ stack ]────
0xffffca80│+0x00: 0x0000007b ("{"?)      ← $esp  # hex(123) = 0x7b
0xffffca84│+0x04: 0x01c8                         # hex(456) = 0x1c8
0xffffca88│+0x08: 0x0804a024  →  0x00            # &sum
0xffffca8c│+0x0c: 0x0804a028  →  0x00            # &product
0xffffca90│+0x10: 0x08048490  →  <__libc_csu_init+0> push ebp
0xffffca94│+0x14: 0x00
0xffffca98│+0x18: 0x00   ← $ebp
0xffffca9c│+0x1c: 0xf7e2cad3  →  <__libc_start_main+243> mov DWORD PTR [esp], eax
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ code:i386 ]────
   0x8048442  <main+5>  lock sub esp, 0x10
   0x8048446  <main+9>  mov DWORD PTR [esp+0xc], 0x804a028
   0x804844e  <main+17>  mov DWORD PTR [esp+0x8], 0x804a024
   0x8048456  <main+25>  mov DWORD PTR [esp+0x4], 0x1c8
   0x804845e  <main+33>  mov DWORD PTR [esp], 0x7b
 → 0x8048465  <main+40>  call 0x804841d <f1>
   ↳  0x804841d  <f1+0>  push ebp
      0x804841e  <f1+1>  mov ebp, esp
      0x8048420  <f1+3>  mov eax, DWORD PTR [ebp+0xc]
      0x8048423  <f1+6>  mov edx, DWORD PTR [ebp+0x8]
      0x8048426  <f1+9>  add edx, eax
      0x8048428  <f1+11>  mov eax, DWORD PTR [ebp+0x10]
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ threads ]────
[#0] Id 1, Name: "9.1x32", stopped, reason: SINGLE STEP
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────[ trace ]────
[#0] RetAddr: 0x8048465, Name: main()
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
gef➤  
```



## ARM 32bit

```
gef➤  disas main
Dump of assembler code for function main:
   0x00008494 <+0>:     push    {r11, lr}
   0x00008498 <+4>:     add     r11, sp, #4
   0x0000849c <+8>:     mov     r0, #123        ; 0x7b
   0x000084a0 <+12>:    mov     r1, #456        ; 0x1c8
   0x000084a4 <+16>:    ldr     r2, [pc, #40]   ; 0x84d4 <main+64>
   0x000084a8 <+20>:    ldr     r3, [pc, #40]   ; 0x84d8 <main+68>
   0x000084ac <+24>:    bl      0x8444 <f1>
   0x000084b0 <+28>:    ldr     r3, [pc, #28]   ; 0x84d4 <main+64>
   0x000084b4 <+32>:    ldr     r2, [r3]
   0x000084b8 <+36>:    ldr     r3, [pc, #24]   ; 0x84d8 <main+68>
   0x000084bc <+40>:    ldr     r3, [r3]
   0x000084c0 <+44>:    ldr     r0, [pc, #20]   ; 0x84dc <main+72>
   0x000084c4 <+48>:    mov     r1, r2
   0x000084c8 <+52>:    mov     r2, r3
   0x000084cc <+56>:    bl      0x82e0
   0x000084d0 <+60>:    pop     {r11, pc}
   0x000084d4 <+64>:    andeq   r1, r1, r12, lsr #32
   0x000084d8 <+68>:    andeq   r1, r1, r0, lsr r0
   0x000084dc <+72>:    andeq   r8, r0, r4, asr r5
End of assembler dump.
```

```
gef➤  disas f1
Dump of assembler code for function f1:
   0x00008444 <+0>:     push    {r11}           ; (str r11, [sp, #-4]!)
   0x00008448 <+4>:     add     r11, sp, #0
   0x0000844c <+8>:     sub     sp, sp, #20
   0x00008450 <+12>:    str     r0, [r11, #-8]
   0x00008454 <+16>:    str     r1, [r11, #-12]
   0x00008458 <+20>:    str     r2, [r11, #-16]
   0x0000845c <+24>:    str     r3, [r11, #-20]
   0x00008460 <+28>:    ldr     r2, [r11, #-8]
   0x00008464 <+32>:    ldr     r3, [r11, #-12]
   0x00008468 <+36>:    add     r2, r2, r3
   0x0000846c <+40>:    ldr     r3, [r11, #-16]
   0x00008470 <+44>:    str     r2, [r3]
   0x00008474 <+48>:    ldr     r3, [r11, #-8]
   0x00008478 <+52>:    ldr     r2, [r11, #-12]
   0x0000847c <+56>:    mul     r2, r3, r2
   0x00008480 <+60>:    ldr     r3, [r11, #-20]
   0x00008484 <+64>:    str     r2, [r3]
   0x00008488 <+68>:    add     sp, r11, #0
   0x0000848c <+72>:    ldmfd   sp!, {r11}
   0x00008490 <+76>:    bx      lr
End of assembler dump.
```





