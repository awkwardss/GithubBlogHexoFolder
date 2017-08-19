---
title: Add Two Numbers
date: 2017-08-19 23:01:32
tags: leetcode
---

## 链表两数相加

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

### SOL

难点在于
* 链表的使用
* 结构体中值和方法的调用
* 对于加法各种情况的处理

```java
/**
 * Definition for singly-linked list.
 * public class ListNode { <-注意这里是list node而非linked list
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */

public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode resultList = new ListNode(0);//由于不能new空node，所以任意赋值，返回时从下一个节点返回值即可
        ListNode t1 = l1;
        ListNode t2 = l2; 
        //ListNode t1 = l1, t2 = l2; 
        ListNode t3 = resultList;
        int digitSum = 0;
        while(t1 != null || t2 !=null){
            if (t1!=null){
                digitSum =digitSum + t1.val; //digitSum += t1.val
                t1=t1.next;//错误写法：t1=t1.next()
            }
            if(t2!=null){//注意t2和t1不要写成嵌套，是错误的逻辑
                    digitSum += t2.val;
                    t2=t2.next;
            } 
            t3.next = new ListNode(digitSum % 10);//这里要记得取余 注意这句是赋值给.next，且new了一个新node并赋值
            t3 = t3.next;
            digitSum = digitSum / 10; //传到下一位的进位，取商 digitSum/=10;
        }
        //别忘了最后一位carry上1的情况！
        if (digitSum == 1){
            t3.next = new ListNode(1);
        }
        return resultList.next;//这里直接把被copy的resultList.next返回，实际在队列中运算的是t3
    }
}
```

