#
"You found a text file with some really low level code. 
Some value at the beginning has been X'ed out. Can you figure out what should be there, 
to make main return the value 0x1? Submit the answer as a hexidecimal number, with no extraneous 0s. 
For example, the decimal number 2015 would be submitted as 0x7df, not 0x000007df."
#
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


# More in depth:
```
main:
    mov $XXXXXXX, %eax
    mov $0, %ebx
    mov $0x5, %ecx
 ```
 First, put a value (XXXXXXX) into the register eax.
 Then, put a value (0) into the register ebx.
 Finally, put a value (5) into the register ecx.
 
 ```
 loop:
    test %eax, %eax
    jz fin
    add %ecx, %ebx
    dec %eax
    jmp loop
  ```
  First, test if eax is zero.
  If so, jump to fin.
  Otherwise, add ecx to ebx.
  Then, decrement eax.
  Jump back to the beginning of the loop.
  
  ```
  fin:
    cmp $0x65c7, %ebx
    je good
    mov $0, %eax
    jmp end
    ```
 Compare ebx to 0x65c7.
 If they are equal, jump to the "good" function.
 Otherwise put the value 0 into eax and jump to the end.
 
 ```
 good:
    mov $1, %eax
end:
    ret
    ```
  If the value in ebx was equal to 0x65c7, good will place the value 1 into eax then return 1.  Otherwise, it will return 0.
