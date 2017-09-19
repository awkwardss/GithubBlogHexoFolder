---
title: Reverse Integer
date: 2017-09-19 09:17:16
tags: leetcode
---

## 7. Reverse Integer 整数取反

Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

click to show spoilers.

Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

Note:
The input is assumed to be a 32-bit signed integer. Your function should return 0 when the reversed integer overflows.

### SOL

稍微处理一下即可, 详见代码

```java
//32 bit int max = 2,147,483,647
//直接存储过大的数会报错, 但是对int做加法不会直接报错，而是变成其他值
//无需对负数单独处理
class Solution {
    public int reverse(int x) {
        //boolean minus = false;
        //if (x<0) minus = true;
        int result=0;
        while(x!=0){
            int tail = x%10; 
            int newResult = result*10+tail;
            if((newResult-tail)/10!=result) return 0;
            result = newResult;
            x=x/10;
        }
        return result;
    }
}
```
