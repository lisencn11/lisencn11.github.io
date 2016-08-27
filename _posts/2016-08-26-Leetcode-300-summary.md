---
layout: post
title: Leetcode Problem 300 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,  
Given [10, 9, 2, 5, 3, 7, 101, 18],  
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

Follow up: Could you improve it to O(n log n) time complexity?

# 我的算法

思路与 334 题类似，我们所需要的就是不断维持 List，其中是截至目前的最长递增子序列，当遇到一个新的数 a<sub>n</sub> 的时候，如果 a<sub>n</sub> 大于 List 尾部元素，直接加入 List；如果小于就替换 List 中相应位置的元素为当前元素，这样并不会影响胃部元素的选择，只会影响对应 index + 1 的元素选择。

# 代码

{% highlight java %}
public class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (list.isEmpty() || nums[i] > list.get(list.size() - 1)) list.add(nums[i]);
            else {
                int index = Collections.binarySearch(list, nums[i]);
                if (index < 0) {
                    index = -(index + 1);
                    list.set(index, nums[i]);
                }
            }
        }
        
        return list.size();
    }
}
{% endhighlight %}