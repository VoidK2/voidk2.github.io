---
layout: post
title: 	HDU 1001 Sum Problem Java
subtitle: Java 练习题
date:  2017-09-21
author: zhangzexin
header-img:  img/post-bg-unix-linux.jpg
catalog: true
tags:
    - Java
    - 练习
---
```
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        int sum,n;
        Scanner in=new Scanner(System.in);
        while(in.hasNextInt()){
            sum=0;
            n=in.nextInt();
            if(n==0){
                System.out.println(sum);
                System.out.println();
                continue;
            }
            if(n%2==0)
                sum=n/2*(n+1);
            else
                sum=(n+1)/2*n;
            System.out.println(sum);
            System.out.println();
        }
    }
}
```
