---
title: Longest Palindromic Substring
date: 2017-08-31 15:45:05
tags: leetcode
---

## 5. Longest Palindromic Substring 最长回文子字符串

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:

Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
Example:

Input: "cbbd"

Output: "bb"

* 单个字符也会返回回文



### SOL

#### 解法1
暴力搜索, 直接对字符串中每个字符左右搜索, 每次搜索分别会搜索奇数和偶数两种情况, 复杂度O(n2).

一些tip:
1. substring的范围是[startIndex, endIndex-1], 分场令人悲愤
2. 使用字符串时要时刻注意下标越界的情况, 会直接返回error --> <b>而如果想防止越界, 也可以给首位再加上符号</b>
3. 在字符串每个字符中间插入一个特殊符号, 可以一次处理奇偶两种情况, 会更加方便. (这也是Manache方法的核心之一)
4. 看上去很简单的暴力算法实现时也是有很多地方需要注意的, 而且出现数组/字符串/多对象时, index的加减运算一定要注意, 不要越界
5. 2k之类的是不对的, 乘法要写作2*k (内心崩溃, 感到自己的傻)


```java
/*法1：暴力枚举*/
class Solution {
    public String longestPalindrome(String s) {
        int i= 0;
        int j =0;
        String result = "";
        if(s.length()<3) return s;//boundary
        while(i<s.length()-1){
            //even//没考虑到ccc这种情况 修正一下
                while(i-j>=0 && i+j+1<s.length()){
                    if(s.charAt(i-j)==s.charAt(i+1+j)){
                        j++;
                     } else{
                        break;
                    }
                }
                if(result.length()<2*j){
                    result = s.substring(i-j+1, i+j+1);
                    //尼玛substring范围竟然是[startIndex, endIndex-1] 吐血 查半天bug
                }         
            j=0;//清零
             //else //odd
                while(i-j>=0 && i+j<s.length()){
                    if(s.charAt(i-j)==s.charAt(i+j)){
                        j++;
                     } else{ 
                        break;
                    }
                }
                if(result.length()<2*j-1){
                    result = s.substring(i-j+1, i+j);
                }       
            
            i++; j=0;
            }
            
        return result;
    }
}
```

### 解法2
解法1的复杂度超时, 故开始写解法2. 查询到名为Manacher算法([马拉车算法](https://www.akalin.com/longest-palindrome-linear-time)<del>这个翻译-_,-</del>)的方法, 复杂度O(n)

算法有三个核心:
1. 在每个字符中间加上特殊符号来合并对于奇偶中心对偶回文的处理, 同时增加一个同长度的数组来存储各字符可生成的最长回文串半径.
2. 算法的核心是根据大回文中轴线两侧若出现小回文,一定会在对侧有同样的小回文这一原理, 来减少不必要的回文字符检查数量, 加快确定最大回文串的中心
3. 在循环过程中, 对于已知最大回文串中心右侧从第一个字符到此串最右端最后一个字符, 中间的每个字符, 判定各个字符的回文串半径初始值的预估方法. 即根据已知最大回文串最右端字符位置和此字符位置的大小判定, 有三种可能: 
    - 最右字符位置<=此字符位置, 超出最大串范围, 直接对初始值赋0;
    - 最右字符位置>此字符位置,未超出最大串范围, 有两种可能. 
        i: (最右字符位置-此字符位置)<此字符以已知最大回文串中心为轴作轴对称得到的字符回文串半径, 即此字符到最右字符的距离比较短, 不够形成和轴对称字符完全相等长度的对称串, 此时初始半径值=(最右字符位置-此字符位置);
    - 最右字符位置>此字符位置,未超出最大串范围. 
        ii: (最右字符位置-此字符位置)>=此字符以已知最大回文串中心为轴作轴对称得到的字符回文串半径, 即此字符到最右字符的距离比较长, 足够形成和轴对称字符完全相等长度的对称串, 此时初始半径值=此字符以已知最大回文串中心为轴作轴对称得到的字符回文串半径.
    - 在给回文串半径赋好初始值之后, 继续进行初始值+1, +2....的判定处理直到找到最大值. 之后若此字符半径最大值大于已知最大值, 那么更新最大回文中心的位置到此字符, 并延展最大回文串最右端的位置.
读起来比较拗口, 不过代码实现实际上比较简单. 马拉车算法对于字符串子回文串的处理非常全面, 可以处理很多回文串的相关问题.

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.length()<2) return s;//记住处理边界条件=L-
        String pre = preProcess(s);
        return manacherProcess(pre, s);
    }

    //预处理输入字符串
    public String preProcess(String s){
        String sout = "$#";
        for(int i=0;i<s.length();i++){
            sout = sout+s.charAt(i)+"#";
        }
        sout+="&";//这里最好和首字符不一样，不然处理时会处理不掉
        return sout;
    }

    public String manacherProcess(String s, String formal){
        int nowMaxCenter = 0;//注意这里和后面初始值是0,特别是后面的,不然会越界
        int nowMaxRight = 0;
        int[] palinRadius = new int[s.length()];
        String result;

        for(int i=1; i<s.length()-1; i++){
            palinRadius[i] = nowMaxRight > i ? Math.min(nowMaxRight-i, palinRadius[2*nowMaxCenter-i]) : 0;

            while(s.charAt(i+palinRadius[i]+1) == s.charAt(i-palinRadius[i]-1)){
                palinRadius[i]++;
            }

            if(palinRadius[i]>palinRadius[nowMaxCenter]){
                nowMaxCenter = i;
                nowMaxRight = i+palinRadius[i];
            }
        }
        
        //这段是不需要的, 因为上面for循环结束时一定会得到最大的回文串之中心与半径.
        //for(int i=1; i<palinRadius.length()-1; i++){
        //    int maxValue = 0;
        //    maxValue = Math.max(palinRadius[i], maxValue);
        //}
        
        //返回结果
        //下面这两行去掉了, 会超时(估计是replace函数导致的), 直接用倒数第二行直接返回的结果
        //result = s.substring(nowMaxCenter-palinRadius[nowMaxCenter], nowMaxCenter+palinRadius[nowMaxCenter]+1);
        //return result.replace("#", "");//记得最后去掉“#”！
        
        result = formal.substring((nowMaxCenter-palinRadius[nowMaxCenter]-1)/2, (nowMaxCenter+palinRadius[nowMaxCenter]-1)/2);
        return result;
    }

}
```

关于最后那行, 根据处理过的字符串求原字符串中的子串位置, 大概解释一下为什么会是[(中心位置-半径-1)/2, (中心位置+半径-1)/2)
首先, 假设原字符串位置是从0,1,2,...i,i+1,....n-1, 被处理过的字符串分别在首尾加了"$#"和"#&" 然后在原字符串中间插入字符"#", 假设新字符串位置为0,1,2,...,j,j+1,...,2n+3.
那么原字符串对应新字符串的关系是: j = 2*(i+1), 但是实际上新字符串获得的子串首尾是"#"符号, 串首"#"符号的位置要+1才是正确的起点, 串尾"#"位置-1才是正确的终点.
故对于首位位置i对应新字符串位置j: ((j+1)/2)-1=i => i=(j-1)/2, j=中心位置-半径 => 首位位置=(中心位置-半径-1)/2;
对于尾位位置i对应新字符串位置j: ((j-1)/2)-1=i, 又因为substring右端index是不包含的,所以还要再给i+1, 最后函数才会取到位置(否则是i-1位置),则给i+1=(j-1)/2, j=中心位置-半径 => 尾位位置=(中心位置+半径-1)/2.

### 解法3
动态规划方法, [一个关于动规的解说链接](http://www.hawstein.com/posts/dp-novice-to-advanced.html)
[参考答案链接](https://segmentfault.com/a/1190000003914228)

睡觉去了, 明天写<del>病了一周的孱弱的胖子留</del>
