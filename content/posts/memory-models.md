+++
title = 'Memory Models'
date = 2024-10-12T20:42:23+08:00
draft = false
+++

## 一些链接

```C
#include <stdio.h>
#include <threads.h>
#include <stdatomic.h>
 
atomic_int acnt;
int cnt;
 
int f(void* thr_data)
{
    for(int n = 0; n < 1000; ++n) {
        ++cnt;
        ++acnt;
        // for this example, relaxed memory order is sufficient, e.g.
        // atomic_fetch_add_explicit(&acnt, 1, memory_order_relaxed);
    }
    return 0;
}
 
int main(void)
{
    thrd_t thr[10];
    for(int n = 0; n < 10; ++n)
        thrd_create(&thr[n], f, NULL);
    for(int n = 0; n < 10; ++n)
        thrd_join(thr[n], NULL);
 
    printf("The atomic counter is %u\n", acnt);
    printf("The non-atomic counter is %u\n", cnt);
}
```

1. [Compiler Testing of C11 Atomics for Arm and RISC-V](https://uu.diva-portal.org/smash/get/diva2:1717258/FULLTEXT01.pdf)
2. [Memory Models](https://research.swtch.com/mm)