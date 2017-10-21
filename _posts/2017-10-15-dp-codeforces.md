---
layout:  post
title:  codeforces 861
subtitle:  整数非重复划分
data:  2017-10-15
author:  Zexin Zhang
header-img:  img/post-bg-ioses.jpg
catalog:  true
tags:
- dp
- codeforces
---
## 整数非重复划分dp
将N分为若干个不同整数的和，有多少种不同的划分方式，例如：n = 6，{6} {1,5} {2,4} {1,2,3}，共4种。由于数据较大，输出Mod 10^9 + 7的结果即可。
Input
输入1个数N(1 <= N <= 50000)。
Output
输出划分的数量Mod 10^9 + 7。
Sample Input
6
Sample Output
4
```c++
//整数非重复划分
#include <cstdio>
#include <cstring>
using namespace std;
int dp[50010][400];
#define m 10^9+7
int main(int argc, char *argv[]){
    int n;
	
	memset(dp,0,sizeof(dp));
	scanf("%d",&n);
	dp[0][0]=1;
		for(int i=1;i<=n;i++)
		for(int j=1;j*j<=i*2;j++){
			dp[i][j]=(dp[i-j][j]+dp[i-j][j-1])%m;
			//printf("dp[%d][%d]=%d",i,j,dp[i]]j);
		}
		//printf("\n");
		int ans=0;
		for(int i=1;i<=n;i++){
			ans+=dp[n][i];
			ans=ans%m;
		}
		printf("%d",ans);
		return 0;
}
```
## codeforces 861a
```c++
//codeforces 861a
#include <iostream>  
#include <stdio.h>  
#include <string.h>  
#include <algorithm>  
#include <cmath>  
using namespace std;  
int main()  
{  
    long long int n,k;  
    while(cin>>n>>k){  
        int x=0,y=0;  
        while(n%5==0&&x<k){  
            n=n/5;  
            x++;  
        }  
        while(n%2==0&&y<k){  
            n=n/2;  
            y++;  
        }  
        for(int i=0;i<k;i++){
        	n=n*10;  
        }  
        cout<<n<<endl;  
    }  
    return 0;  
}  
```
## codeforces 861c
```c++
//codeforces 861c
#include <cstdio>
#include <cstring>
using namespace std;
#define maxn 3005

char s[maxn];
char str[]="aeiou";

bool judge(char c){
    for(int i=0;i<5;i++){
        if(c==str[i])
            return false;
    }
    return true;
}

int main()
{
    scanf("%s",s);
    int len=strlen(s);
    int pre=-1;
    for(int i=0;i<len;i++){
        putchar(s[i]);
        if(i!=len-1&&pre<=i-2&& judge(s[i]) && judge(s[i-1]) && judge(s[i+1])&& (s[i]!=s[i-1]||s[i]!=s[i+1]||s[i-1]!=s[i+1]) ){//判断三个字符不相同且为元音 
            putchar(' ');
            pre=i;
        }
    }
    puts("");
    return 0;
}
```
