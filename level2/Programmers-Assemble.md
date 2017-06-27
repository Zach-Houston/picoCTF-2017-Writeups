
"You found a text file with some really low level code. 
Some value at the beginning has been X'ed out. Can you figure out what should be there, 
to make main return the value 0x1? Submit the answer as a hexidecimal number, with no extraneous 0s. 
For example, the decimal number 2015 would be submitted as 0x7df, not 0x000007df."

```
.global main

main:
    mov $XXXXXXX, %eax
    mov $0, %ebx
    mov $0x5, %ecx
loop:
    test %eax, %eax
    jz fin
    add %ecx, %ebx
    dec %eax
    jmp loop
fin:
    cmp $0x65c7, %ebx
    je good
    mov $0, %eax
    jmp end
good:
    mov $1, %eax
end:
    ret

```

Looking at this code, we can see that ebx = 0 and ecx = 5 in main.
The loop tests when eax is zero.  The loop also adds 5 to ebx each loop.  

Equivalent loop:
```
for( int eax = XXXXX; eax != 0; eax-- ){
  ebx = ebx - ecx
}
```

After the loop, ebx is compared to 0x65c7 to determine if they are equal.
0x65c7 = 26,055, so eax = 26,055/5 = 5211.  
In hex, this is 0x145b, giving us the flag.