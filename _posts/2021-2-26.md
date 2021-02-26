---
layout:  post
title: leetcode刷题
subtitle: 记录
date: 2021-2-26
author: zhangzexin
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
- leetcode
- java
---

## 1178 [猜字谜](https://leetcode-cn.com/problems/number-of-valid-words-for-each-puzzle/)

```java
class Solution {
    public List<Integer> findNumOfValidWords(String[] words, String[] puzzles) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(String word:words){
            int mask=getBit(word);
            if(Integer.bitCount(mask)<=7){
                map.put(mask,map.getOrDefault(mask,0)+1);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for(String puzzle:puzzles){
            int cnt=0;
            int first=getBit(puzzle.substring(0,1));
            int mask=getBit(puzzle.substring(1));
            int subset=mask;
            while(subset!=0){
                int key=first|subset;
                if(map.containsKey(key)){
                    cnt+=map.get(key);
                }
                subset=(subset-1)&mask;
            }
            if(map.containsKey(first)){
                cnt+=map.get(first);
            }
            ans.add(cnt);
        }
        return ans;
    }
    private int getBit(String word){
        int mask=0;
        for(int i = 0; i < word.length(); i++){
            char ch=word.charAt(i);
            mask |=( 1 << ( ch - 'a' ));
        }
        return mask;
    }
}
```
