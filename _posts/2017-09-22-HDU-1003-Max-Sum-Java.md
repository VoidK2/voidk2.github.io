---
layout:     post
title:      HDU 1003 Max Sum Java
subtitle:   HDU 1003 JAVA
date:       2017-09-22
author:     zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog:    true
tags:
    - HDU
    - Java
---
```

import java.util.Scanner;
public class Main{
	public static void main(String[] args){
		int _case,n,i;
		int []a=new int [100100];
		int []b=new int [100100];
		int []c=new int [100100];
			Scanner in=new Scanner(System.in);
			_case=in.nextInt();
			int js=0;
			while(_case-->0){
				js++;
				n=in.nextInt();
				for(i=0;i<n;i++){
					a[i]=in.nextInt();
				}
				b[0]=a[0];
				c[0]=0;
				for(i=1;i<n;i++){
					if(b[i-1]+a[i]<a[i]){
						b[i]=a[i];
						c[i]=i;
					}
					else{
						b[i]=b[i-1]+a[i];
						c[i]=c[i-1];
					}
				}
				int ed=0;
				int max=b[0];
				for(i=1;i<n;i++){
					if(max<b[i]){
						max=b[i];
						ed=i;
					}
				}
				System.out.println("Case "+js+":");
				System.out.println(max+" "+(c[ed]+1)+" "+(ed+1));
				if(_case>0)
				System.out.println();
			}
	}
}
```
