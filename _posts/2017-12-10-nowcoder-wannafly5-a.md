---
layout:  post
title:  wannafly5-a
subtitle:  正误a题
data:  2017-12-10
author:  Zexin Zhang
header-img: img/post-bg--unix-linux.jpg  
catalog:  true
tags:
- acm
- algorithm
---
## 题目
![](https://upload.cc/i/PtMRHw.png)
**自己超时的代码**
```c++
#include <cstdio>

#include <cmath>

#include <omp.h>

using namespace std;
int sqrt1(int x) { // 牛顿迭代

	if (x == 0) return 0;
	double last = 0;
	double res = 1;
	while (res != last)
	{
		last = res;
		res = (res + x / res) / 2;
	}
	return int(res);
}
bool isqual(int s)
{
	int sqrt12 = sqrt1(s);
	if (s % 10 == 0 || s % 10 == 1 || s % 10 == 4 || s % 10 == 5 || s % 10 == 6 || s % 10 == 9) {
		if (sqrt12 * sqrt12 == s)
			return true;
		else
			return false;
	}
	else
		return false;
}

int main() {
	int n;
	int a[100001];
	int sum = 0;
	long long ans = 0;
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &a[i]);
	}
	#pragma omp parallel for
	for (int i = 1; i <= n; i++) {
		for (int j = i; j <= n; j++) {
			sum += a[j];
			if (isqual(sum))
				ans++;
		}
		sum = 0;
	}
	printf("%lld", ans);
	return 0;
}
```
