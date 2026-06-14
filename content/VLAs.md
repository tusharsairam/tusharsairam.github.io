---
title: VLAs in C
---
## Links
https://zakuarbor.github.io/blog/variable-len-arr/
http://ayekat.ch/blog/vla
## Notes
VLAs (Variable Length Arrays) have gained compiler support since C99
```c
#include <stdio.h>
int main()
{
    int x = 5;
    int arr[x]; //<-- VLA declaration 
}
```

* VLAs are inefficient due to extra overhead because the compiler has to decide the size of the array and allocate stack memory Unless standard arrays where the size is known during compile-time
* I learnt that VLAs follow automatic storage duration - It means that the array is purely local-scope. For example, if a VLA is declared inside a for loop, it cannot be accessed outside the loop

They're good only if you want to create small arrays and you're confident that a stack overflow isn't going to happen with that size

For me, I fell back to using VLAs because on embedded C, I couldn't do heap allocation using `malloc` because that's typically not in good taste as `malloc` may introduce unwanted behaviour and the size of the array cannot be determined at compile-time. Perhaps what I can do is to overcompensate the size a little bit

For allocating a 2D matrix, the `m/calloc` alternative is
```c
// assume stdlib.b is included
size_t n;
int (*mat)[n] = calloc(n * sizeof(*mat));
```


