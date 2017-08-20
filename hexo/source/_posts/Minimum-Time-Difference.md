---
title: Minimum Time Difference
date: 2017-08-20 16:55:59
tags: leetcode
---

## 539. Minimum Time Difference 数列内最小时间差

Given a list of 24-hour clock time points in "Hour:Minutes" format, find the minimum minutes difference between any two time points in the list.

Example 1:
```
    Input: ["23:59","00:00"]
    Output: 1
```
Note:
The number of time points in the given list is at least 2 and won't exceed 20000.
The input time is legal and ranges from 00:00 to 23:59.

### SOL

没看到note里面就开始想题,还以为是只有两个值的我写到一半发现写错,简直崩溃……(所以一定要<b>认 真 审 题</b>

#### 解法1
复杂度: nlog(n) 主要是sort的复杂度

思路：
* 提取出时间后直接变成分钟数，分钟差>12*60时直接给小时间+24*60，再做大时间-小时间的减法
* 只需要对两个相邻时间进行减法，因为需要的是最短时间=！=，这个trick我竟然是看了答案才想到，还以为要分类计算啥的，说明有时候想的太复杂了(<del>或者就是傻</del>

总结:
* 认真审题
* 时间(list)向数字的转换
* 集合(各种list)的使用还是要加强，不能每个方法都要现搜

一些小的注意点:
1. 在生成ArrayList的时候，ArrayList<int>是<b>错误</b>的, https://goo.gl/g9vo6m 指出应使用这种格式

```
List<Integer> xxx = new ArrayList<Integer>();
```
   (注意左侧是list,<>里面是Integer,右边的<Integer>可写可不写
2. 分割String时使用的split()方法
3. arrayList获取数组长度时用的是size（）非length（）; 获取arraylist元素使用.get（）


```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        List<Integer> timeList = new ArrayList<>(); //<-创建集合存放所有时间 
        int time, timeDiff=12*60+1;
        String[] splitedTime;
        //读取所有时间->int arraylist中
        for (String s: timePoints){
            splitedTime = s.split(":"); //<-split函数
            time = (Integer.parseInt(splitedTime[0]))*60+Integer.parseInt(splitedTime[1]); //<-String to Int
            timeList.add(time);
        }
        Collections.sort(timeList);
        for(int i=0;i<timeList.size()-1;i++){ //<-注意arrayList用的是size（）非length（）
            int diff = Math.abs(timeList.get(i)-timeList.get(i+1));//获取arraylist中数使用.get（）
            if (diff<timeDiff){
                timeDiff = diff;
            }
        }
        int a=Math.abs(timeList.get(0)+24*60-timeList.get(timeList.size()-1));
        if(a<timeDiff){
        timeDiff = a;
        }
        return timeDiff;
    }
}
```

#### 解法2

解法2 (https://goo.gl/ptB4F9 )就更简洁一些了. 因为时间数值只有24*60即1440个, 所以建立一个1440的boolean数组来标示是否出现各个时间点即可 (斯国一.jpg

复杂度: O(n)

注意:
* 别忘了处理重复值的情况！！！(眼神放空
* 比较大小用Math.min/max更配哦(((
* length: array用(int[], string[]); 
  length(): String相关Obj用(String, StringBuilder, etc); 
  size(): Collection Obj用(ArrayList, Set, etc) via https://goo.gl/arGP91

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        boolean[] array = new boolean[24*60];
        int pre = Integer.MAX_VALUE;
        int first = Integer.MAX_VALUE;
        int last = Integer.MIN_VALUE;
        int timeDiff = Integer.MAX_VALUE;
        //time String to array
        for(String s: timePoints){
            String[] time = s.split(":");
            int hour = Integer.parseInt(time[0])*60;
            int minute = Integer.parseInt(time[1]);
            //!!!!!!!!!!!!!!!!!!!!!!!!!别忘了处理重复值的情况！！！（苦痛脸
            if (array[hour + minute]){
                return 0;
            }
            array[hour + minute] = true;
        }
        
        //compare
        for(int i=0;i<array.length;i++){ 
            if(array[i]){
                if(pre<Integer.MAX_VALUE){
                   //if(timeDiff>(i-pre)){
                   //    timeDiff = i-pre;
                   //}//用Math.min更简洁！
                    timeDiff = Math.min(timeDiff, i-pre);
                }
                pre = i;
                first = Math.min(first, i);
                last = Math.max(last, i);
                //if(i<first){//同上
                //    first = i;
                //}
                //if(i>last){//同上
                //    last = i;
                //}
            }
        }
        int gap = first + 24*60 - last;
        //if(gap<timeDiff){
        //    timeDiff = gap;
        //}
        timeDiff = Math.min(gap, timeDiff);
        return timeDiff;
    }
}
```