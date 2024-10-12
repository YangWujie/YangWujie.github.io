+++
title = '编译器优化'
date = 2024-10-12T22:03:55+08:00
draft = false
+++

## 为什么打印不出 1？

```C
#include <stdio.h>
#include <threads.h>

int x = 0;
int done = 0; 
 
int f1(void* thr_data)
{
    while (done == 0);
    printf("%d\n", x);

    return 0;
}

int f2(void* thr_data)
{
    x = 1;
    done = 1;

    return 0;
}

int main(void)
{
    thrd_t thr1, thr2;
    thrd_create(&thr1, f1, NULL);
    thrd_create(&thr2, f2, NULL);

    thrd_join(thr1, NULL);
    thrd_join(thr2, NULL);

    return 0;
}
```

```
gcc opt.c -O3 -o opt
```