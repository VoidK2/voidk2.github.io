---
layout:  post
title: HackerRank初试
subtitle: 几道题
date: 2018-06-13
author: zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - algorithm
---
## A
Complete the function solveMeFirst to compute the sum of two integers.

**Function prototype:**

int solveMeFirst(int x, int y);

where,

- x is the first integer input.
- y is the second integer input

**Return values**

sum of the above two integers

**Sample Input**

> x = 2
> y = 3

**Sample Output**

> 5

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

int solveMeFirst(int a, int b) {
    return a+b;
}

int main() {
    int num1,num2;
    scanf("%d %d",&num1,&num2);
    int sum; 
    sum = solveMeFirst(num1,num2);
    printf("%d",sum);
    return 0;
}
```
