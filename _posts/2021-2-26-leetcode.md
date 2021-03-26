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

## 1178.[猜字谜](https://leetcode-cn.com/problems/number-of-valid-words-for-each-puzzle/)

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

## 1.[两数之和](https://leetcode-cn.com/problems/two-sum/)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] ans = new int[2];
        label1:for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i]+nums[j]==target){
                    ans[0]=i;
                    ans[1]=j;
                    break label1;
                }
            }
        }
        return ans;
    }
}
```

## 2.[两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int a1=0,a2=0,a3=0,tmp=0,times=1;
        ListNode l3=new ListNode();
        ListNode l4=l3;
        while(l1!=null||l2!=null||tmp!=0){
            int x=l1==null?0:l1.val;
            int y=l2==null?0:l2.val;
            int sum=x+y+tmp;
            tmp=sum/10;
            l4.next=new ListNode(sum%10);
            l4=l4.next;
            if(l1!=null){
                l1=l1.next;
            }
            if(l2!=null){
                l2=l2.next;
            }
        }
        return l3.next;
    }
}
```

## 201.[数字范围按位与](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        if(right==0){
            return 0;
        }
        int i=0;
        while(left!=right){
            left=left>>1;
            right=right>>1;
            i++;
        }
        return left<<i;
    }
}
```


## 82.[删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null){
            return head;
        }
        int flag=0;
        ListNode head2 = new ListNode(0,head);
        ListNode head3=head2;

        while(head3.next!=null&&head3.next.next!=null){
            if(head3.next.val==head3.next.next.val){
                flag=head3.next.val;
                while(head3.next!=null&&head3.next.val==flag){
                    head3.next=head3.next.next;
                }
            }else{
                head3=head3.next;
            }
        }
        return head2.next;
    }
}
```

## 83.[删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null){
            return head;
        }
        int flag=0;
        ListNode head2 = new ListNode(0,head);
        ListNode head3=head2;

        while(head3.next!=null&&head3.next.next!=null){
            if(head3.next.val==head3.next.next.val){
                flag=head3.next.val;
                while(head3.next.next!=null&&head3.next.next.val==flag){
                    head3.next=head3.next.next;
                }
            }else{
                head3=head3.next;
            }
        }
        return head2.next;
    }
}
```

## 456.[132 模式](https://leetcode-cn.com/problems/132-pattern/)
```java
class Solution {
    public boolean find132pattern(int[] nums) {
        if(nums.length<3){
            return false;
        }
        int min=nums[0];
        for(int i=0;i<nums.length;i++){
            min=Math.min(min,nums[i]);
            if(min==nums[i]){
                continue;
            }
            for(int j=nums.length-1;j>i;j--){
                if(min<nums[j]&&nums[j]<nums[i]){
                    return true;
                }
            }
        }
        return false;
    }
}
```

## 191.[位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ans=0;
        for(int i=0;i<32;i++){
            if((n&(1<<i))!=0){
                ans++;
            }
        }
        return ans;
    }
}
```
