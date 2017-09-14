---
title: ZigZag Conversion
date: 2017-09-03 11:45:50
tags: leetcode
---

## 6. ZigZag Conversion zigzag模式转换

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".


### SOL

很容易能看出字符串的排列位置是以(2n-2)的间隔重复, 直接循环取值形成每行字符串即可.
一个小注意点在于对于每个循环内部两端对称的字符并不是完全的等差序列, 而是相对于每个循环首尾字符等差, 所以在这个基础上加上一些小判定和处理即可. 思路比较直接. 
复杂度O(n).

实际上实现的难点是各种+1-2....等的处理, 以及小心不要数组越界, 稍有不慎就会加错/越界orz导致整个结果出错.

注意点:
- 边界条件, 对于边界条件的思索和处理应该是第一步, 而不是等到submit/test返回error才想到忘记处理边界了.
- 对于数组内部数字的运算, 想好再写, 不然复杂的处理很容易越界.

```java
class Solution {
    public String convert(String s, int numRows) {
        //别忘了第一件事处理边界！！
        if (s.length()<=1 || numRows<=1) return s;
        String innerPart = "";
        String result = "";
        for(int i=0; i<numRows; i++){//处理
            innerPart="";
            int j=0;
            while(j<(s.length())/(2*numRows-2)){//只处理可整除部分
                innerPart+=s.charAt(i+(2*numRows-2)*j);
                if (i!=0 && i!=numRows-1) innerPart+=s.charAt((2*numRows-2)*(j+1)-i);
                j++;
            }
            //处理余数
            int mod=s.length()%(2*numRows-2);
            if (mod>i) {
                innerPart+=s.charAt((s.length()/(2*numRows-2))*(2*numRows-2)+i);
                if(i!=0 && i!=numRows-1 && ((2*numRows-2)-i) <mod){
                    innerPart+=s.charAt(((s.length()/(2*numRows-2))+1)*(2*numRows-2)-i);
                }    
            }
            result+=innerPart;
            //System.out.print(result);
        }
        return result;
    }
}
```