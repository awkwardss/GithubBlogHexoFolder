---
title: Median of Two Sorted Arrays
date: 2017-08-21 12:56:15
tags: leetcode
---
## 4. 两个有序数列的中位数 Median of Two Sorted Arrays

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
Example 2:
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5

### SOL1
由于两个数列都是有序的，直接merge两个数组再求中间值，复杂度O（n）。

* 注意最后返回值做除法时，不能直接写/2，会自动进行转换变成int，提醒是要注意返回值的格式


```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int i = 0;
        int j = 0;
        int k = 0;
        int[] array = new int[nums1.length+nums2.length];
        while(i<nums1.length && j<nums2.length){
            if(nums1[i]<nums2[j]){
                array[k]=nums1[i];
                k++;
                i++;
            } else {
                array[k]=nums2[j];
                k++;
                j++;
            }
        }
        while(i<nums1.length){
            array[k]=nums1[i];
            k++;
            i++;
        }
        while(j<nums2.length){
            array[k]=nums2[j];
            k++;
            j++;
        }
        //compute median
        if(array.length%2 ==1){
            return array[(int)(array.length/2)];
        } else {
            return (array[(array.length/2)]+array[(array.length/2)-1])/2.0; //！！！这里一定不能写/2，会自动变成int！！！
        }
        
    }
}
```

### SOL2
题目要求里面是需要log(m+n)
