---
layout: post
title: Leetcode Problem 287 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Note:  
1. You must not modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

# 我的算法

审题发现有几点需要注意：

1. 数组中的元素个数和元素范围进行了限定
2. O(1)空间复杂度的情况下，时间复杂度要优于O(n<sup>2</sup>)

对于无序数组的找重，还要求O(1)空间复杂度，时间复杂度肯定要达到O(n<sup>2</sup>)，要优于考虑O(nlogn)，对无序数组本身肯定没什么算法，看来要用在元素范围在 1 到 n 之间这个条件。

O(nlogn)和 1 到 n 有序，我们联想到二分查找。

首先取 mid ，统计数组中 mid 小于等于的元素个数，若小于等于mid，说明重复元素在 mid (exclusive) 到 high 之间，反之在 1 到 mid (inclusive) 之间。

# 代码

{% highlight java %}
public class Solution {
    public int findDuplicate(int[] nums) {
        int low = 1;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            int lessCount = 0;
            for (int i = 0; i < nums.length; i++) {
                lessCount = nums[i] <= mid ? lessCount + 1 : lessCount;
            }
            if (lessCount <= mid) low = mid + 1;
            else high = mid;
        }
        
        return low;
    }
}
{% endhighlight %}

# discuss

discuss 中居然有 O(n) 的算法，惊了个呆。。。

思路和**在含有环的链表中寻找环的起点**问题相同，把数组的每个单元看成一个结点，单元存储的值是下个结点的 index，我们需要 two pointers ，一个快一个慢先碰到，然后再让一个从起点开始，再次碰到的时候就找到了重复元素。

可以使用这个算法是因为限定了取值为 1 到 n。

{% highlight java %}
public class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[nums[0]];
        
        while (fast != slow) {
            fast = nums[nums[fast]];
            slow = nums[slow];
        }
        
        fast = 0;
        while (fast != slow) {
            fast = nums[fast];
            slow = nums[slow];
        }
        return slow;
    }
}
{% endhighlight %}