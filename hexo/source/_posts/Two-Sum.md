---
title: Two Sum
date: 2017-07-24 10:05:39
tags: leetcode
---

## 1. 两数求和问题

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

### SOL

先实现了第一种最简单的解法, 整体遍历, 复杂度O(n2)

```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] sol = new int[2];
        for (int i = 0; i<nums.length; i++){
            for (int j = i+1; j<nums.length;j++){
                if (target == nums[i] + nums[j]){
                    sol[0] = i;
                    sol[1] = j;
                }
            }
        }
    return sol;
    } 
}

```

剩下的hashmap方法晚上再研究一下