---
layout: post
title: Leetcode Problem 4 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

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

# 我的算法

详细解释在[这里](https://discuss.leetcode.com/topic/4996/share-my-o-log-min-m-n-solution-with-explanation)。

简述就是先求两个数组求中位数的左半边加和应该是多少，然后根据  
1. a 数组左半边最大值和 b 数组右半边最小值的关系  
2. b 数组左半边最大值和 a 数组右半边最小值的关系  

来判断 a 数组的指针移动，以及 b 数组的相对移动位置。

特别注意：  
1. 由于我们是先折半 a 数组，再用 a 数组的长度求 b 数组左半边的长度，所以要保证 b 数组比 a 数组长，
2. 一个数组为空的处理
3. 有一个数组左半边长度为 0 或者右半边长度为 0

# 代码

{% highlight java %}
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int halfLen = (m + n) % 2 == 1 ? (m + n + 1) / 2 : (m + n) / 2;
        
        if (m == 0 && n == 0) {
            return 0;
        } else if (m == 0) {
            if (n % 2 == 1) {
                return (double) nums2[n / 2];
            } else {
                return ((double) nums2[n / 2] + (double) nums2[n / 2 - 1]) / 2;
            }
        } else if (n == 0) {
            if (m % 2 == 1) {
                return (double) nums1[m / 2];
            } else {
                return ((double) nums1[m / 2] + (double) nums1[m / 2 - 1]) / 2;
            }
        }
        
        if (m > n) {
            int[] temp1 = nums1;
            int temp2 = m;
            nums1 = nums2;
            nums2 = temp1;
            m = n;
            n = temp2;
        }
        
        int low1 = 0;
        int high1 = nums1.length;

        while (low1 <= high1) {
            int mid1 = low1 + (high1 - low1) / 2;
            int mid2 = halfLen - mid1;
            if ((mid1 == 0 || mid2 == n || nums1[mid1 - 1] <= nums2[mid2]) && (mid2 == 0 || mid1 == m || nums1[mid1] >= nums2[mid2 - 1])) {
                
                double maxOfLeft = 0;
                if (mid1 == 0) maxOfLeft = (double) nums2[mid2 - 1];
                else if (mid2 == 0) maxOfLeft = (double) nums1[mid1 - 1];
                else maxOfLeft = (double) Math.max(nums1[mid1 - 1], nums2[mid2 - 1]);
                
                double minOfRight = 0;
                if (mid1 == m) minOfRight = nums2[mid2];
                else if (mid2 == n) minOfRight = nums1[mid1];
                else minOfRight = (double) Math.min(nums1[mid1], nums2[mid2]);
                
                if ((m + n) % 2 == 1) {
                    return maxOfLeft;
                } else {
                    return (maxOfLeft + minOfRight) / 2;
                }
                
            } else if (mid1 == 0 || mid2 == n || nums1[mid1 - 1] <= nums2[mid2]) {
                low1 = mid1 + 1;
            } else if (mid2 == 0 || mid1 == m || nums1[mid1] >= nums2[mid2 - 1]) {
                high1 = mid1 - 1;
            }
        }
        return 0;
    }
}
{% endhighlight %}