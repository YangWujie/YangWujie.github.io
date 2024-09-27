+++
title = '浮点数相除'
date = 2024-09-27T21:46:54+08:00
draft = false
+++

## 浮点数相除时，如何避免除数为0？

我们刚一学习计算机课程时就被教导，由于计算机系统种浮点数的表示误差和运算误差，直接用相等
运算符判断两个浮点数是否相等是一件危险的事情。请看下面的例子：

```C
#include <stdio.h>

int main()
{
    double a = (0.3 * 3) + 0.1;
    double b = 1;
    if (a == b) {
        printf("The numbers are equal\n");
    }
    else {
        printf("The numbers are not equal\n");
    }
    return 0;
}
```

程序的输出结果为：
```
The numbers are not equal
```

为了避免这种尴尬，我们比较两个浮点数是否相等时，通常需要采取一些措施，
最简单的方法就是允许微小的误差：
```C
#include <stdio.h>
#include <stdlib.h>

int main()
{
    double a = (0.3 * 3) + 0.1;
    double b = 1;
    if (abs(a - b) < 1e-9) {
        printf("The numbers are equal\n");
    }
    else {
        printf("The numbers are not equal\n");
    }
    return 0;
}
```

修改后的程序的输出为：
```
The numbers are equal
```

那么问题来了：当我们将两个浮点数相除时，为了避免除数为0，应该怎么做？
直接拿除数和0比较还是应该谨慎一些？
```C
    if (divisor == 0) 
```
还是
```C
    if (abs(divisor) < 1e-9))
```
更合适？

之所以有此一问是因为[PTA](https://pintia.cn/home)平台上的题库里有一个题目，
用相等运算符通不过，用`abs`函数则能通过。

我认为这个题目显然有问题。0不能做除数，不表示接近于0的数不能做除数。无论一个数有多小，
它只要不是0，它就可以做除数，这一点是显而易见的。例如下面的程序，即便除数小至1e-100，
运算也能得到一个有意义的结果：

```C
#include <stdio.h>

int main()
{
    double a = 1.23e-98;
    double b = 1e-100;
    printf("a / b = %lf\n", a / b);
    return 0;
}
```

程序的输出结果为：
```
a / b = 123.000000
```

所以，我认为判断除数是否为0时，禁用相等运算符是毫无道理的。