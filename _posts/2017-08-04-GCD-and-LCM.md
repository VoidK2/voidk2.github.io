---
layout:  post
title: GCD and LCM(最大公约数最小公倍数模版)
subtitle: 算法模版
date: 2017-08-04
author: zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
  _ algorithm
  - 模版
---
### 最大公约数、最小公倍数模版
第一弹
```
int gcd(int a,int b)  
{  
	if(a%b==0)  
		return b;  
	else  
		return gcd(b,a%b);  
}  

int lcm(int a,int b)  
{  
	return (a*b)/gcd(a,b);  
}
```
第二弹
```
int gcd(int a,int b){
		if(b==0)
				return a;
		return gcd(b,a%b);
}
int lcm(int a,int b){
			return a/gcd(a,b)*b;
		a=ans;
		}

```