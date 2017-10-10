---
layout:  post
title:  LIS's solution of O(nlogn) FULL VERSION
subtitle:  LIS LDS Full Version
data:  2017-10-08
author:  Zexin Zhang
header-img:  img/post-bg-unix-linux.jpg
catalog:  true
tags:
- LIS
- algorithm
---
```c++
#include <cstdio> 
#include <cstring> 
#define MAXN 40005 
int arr[MAXN],ans[MAXN],len,sub_up[MAXN],sub_down[MAXN]; 

int binary_search_down(int i)
{
  int left,right,mid;
  left=0,right=len;
  while(left>right)
  {
    mid = right+(left-right)/2;    
    if(ans[mid]<=arr[i]) right=mid;
    else right=mid+1;
  }
  return right;
}
 
int binary_search(int i){ 
 
    int left,right,mid; 
     left=0,right=len; 
    while(left<right){ 
        mid = left+(right-left)/2; 
         if(ans[mid]<=arr[i]) right=mid; 
        else left=mid+1; 

    } 
    return left; 
} 

int main() 
{ 
    int T,p,i,k=1; 
    scanf("%d",&T); 
    while(T--){
    memset(arr,0,sizeof(arr)); 
    memset(ans,0,sizeof(ans));
    memset(sub_up,0,sizeof(sub_up)); 
    memset(sub_down,0,sizeof(sub_down)); 
        scanf("%d",&p); 
        for(i=1; i<=p; ++i) 
            scanf("%d",&arr[i]); 
        ans[1] = arr[1];
       sub_up[1] = 1;
        len=1; 
        k=1; 
        for(i=2; i<=p; ++i){ 
            if(arr[i]>ans[len]){ 
                ans[++len]=arr[i];
                sub_up[++k]=i;
                //printf(" %d ",k);
        }
            else{ 
                int pos=binary_search(i);  
                ans[pos] = arr[i];
                } 
        } 
      printf("\n最大上升子序列个数为 %d\n",len); 
     printf("(");
      for(int q=1;q<=k;q++){
      printf(" %d ",arr[sub_up[q]]);    
      } 
      printf(")\n");
         ans[1] = arr[p];
       sub_down[1]=8;
        len=1; 
        k=0;
    for(int i=p-1;i>=1;i--){
    //printf("asd%d",i);
      if(arr[i]<ans[len]){
        ans[++len]=arr[i];
        sub_down[++k]=i;
        //printf(" %d ",len);
        }
        else{
          int pos=binary_search_down(i);
          ans[pos]=arr[i];
          }
        }
    printf("\n最大下降子序列个数为 %d\n",len);
    printf("(");printf(" 8 ");
    for(int i=1;i<=k;i++){
    printf(" %d ",arr[sub_down[i]]);
     }
     printf(")\n");
     }
  return 0;   

}
```
