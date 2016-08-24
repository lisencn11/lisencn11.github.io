---
layout: post
title: Leetcode Problem 154 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

The array may contain duplicates.

# 我的算法

与题目 153 类似，但是这里允许重复元素，我的做法是分情况讨论，如果说首尾元素不同，直接套用 153 解法即可，如果首尾元素相同要转化成首尾元素不同的情况，首先 p1 不断向前知道遇到和 p2 不同的值为止

* 若没找到直接返回 p1 的值表示数组中元素全部相同。
* 若 p1 的值小于 p2 的值表示左半边全部相同， p1 当前指向最小值
* 其他情况按照 153 的解法即可

# 代码

{% highlight java %}
public class Solution {
    public int findMin(int[] nums) {
        int p1 = 0;
        int p2 = nums.length - 1;
        
        while (p1 < p2 && nums[p1] == nums[p2]) {
            p1++;
        }
        if (p1 >= p2) return nums[p1];
        if (nums[p1] < nums[p2]) return nums[p1];
        
        int start = p1;
        int end = p2;
        while (p1 < p2) {
            int mid = p1 + (p2 - p1) / 2;
            if (nums[mid] >= nums[start]) p1 = mid + 1;
            if (nums[mid] <= nums[end]) p2 = mid;
        }
        return nums[p1];
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public int findMin(int[] nums) {
	 int l = 0, r = nums.length-1;
	 while (l < r) {
		 int mid = (l + r) / 2;
		 if (nums[mid] < nums[r]) {
			 r = mid;
		 } else if (nums[mid] > nums[r]){
			 l = mid + 1;
		 } else {  
			 r--;  //nums[mid]=nums[r] no idea, but we can eliminate nums[r];
		 }
	 }
	 return nums[l];
}
{% endhighlight %}