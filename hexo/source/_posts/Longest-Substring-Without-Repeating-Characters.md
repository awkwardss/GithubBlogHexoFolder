---
title: Longest Substring Without Repeating Characters
date: 2017-08-21 11:52:47
tags: leetcode
---

## 3. 最长非重复字符串 Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


### SOL

看到题想到用map来实现, 具体思路还是参考了网路上这张图↓
![hint](http://img.blog.csdn.net/20130901224716625)
以及网路上的讲解:
> 假设子串里含有重复字符,则父串一定含有重复字符,单个子问题就可以决定父问题,因此可 以用贪心法。跟动规不同,动规里,单个子问题只能影响父问题,不足以决定父问题。
  从左往右扫描,当遇到重复字母时,以上一个重复字母的index+1作为新的搜索起始位置, 直到最后一个字母,复杂度是 O(n)。

但是在实现的时候还是结果有错, 仔细看了下发现是没有考虑多个重复字符的计算问题(eg: abcdba...), 于是在重复字符出现的判定那里又加上了新的变量j用于标示上一个滑动窗口(好名字)的起点,从而把map中前一个窗口的键值都清除.

*
* map的用法，map在处理字符串的时候感觉很常用
* 字符串的用法
* 这题没什么太大难度，把背后的逻辑想明白就能得出很好的答案

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int start = 0;
        int length = 0;
        for(int i = 0,j=0; i<s.length(); i++){
            if(map.containsKey(s.charAt(i))){
                //if occur repeat?
                length = Math.max(length, i-start);
                start = map.get(s.charAt(i))+1;
                for(int k=j; k<start;k++){
                    map.remove(s.charAt(k));
                }
                j=start;
            }
            map.put(s.charAt(i), i);
        }
        length = Math.max(length, s.length()-start);
        return length;
    }
}
```
<del>写到一半IntelliJ竟然挂了……<del/>

