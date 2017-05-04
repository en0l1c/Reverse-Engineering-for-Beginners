# Chap05. prinf()

## C

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