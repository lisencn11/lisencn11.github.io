---
layout: post
title: Leetcode Problem 128 Summary
date: 2016-08-07
categories: blog
tags: [study]

---

# 题目

对于一个无序整型数组，求其中的最长连续序列。

例如：

For example,  
Given [100, 4, 200, 1, 3, 2],  
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

要求时间复杂度为 O(n)

# 我的算法

如果没有时间复杂度要求，简单的排序后遍历即可，时间复杂度为 O(nlogn) 。要求时间复杂度为 O(n) ，我们可以这样推断，遍历数组寻找最长连续序列时可以做到 O(1) 的时间复杂度找到下一个元素，这种要求只有哈希表可以实现，所以很明显我们需要使用 HashSet 或者 HashMap 来实现。

我的思路是用两个 HashSet 一个纪录数组中存在的元素，用来 O(1) 的查询时间，另一个用来记录访问过的元素以提升效率。

# 代码

{% highlight java %}
public class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        Set<Integer> exist = new HashSet<>();
        for (int i = 0; i < nums.length; i++)
            exist.add(nums[i]);
            
        Set<Integer> used = new HashSet<>();
        int maxLen = 1;
        for (int i = 0; i < nums.length; i++) {
            if (used.contains(nums[i])) continue;
            int len = 1;
            int left = 1;
            int right = 1;
            while (exist.contains(nums[i] - left)) {
                len++;
                used.add(nums[i] - left);
                left++;
            }
            while (exist.contains(nums[i] + right)) {
                len++;
                used.add(nums[i] + right);
                right++;
            }
            maxLen = Math.max(maxLen, len);
        }
        
        return maxLen;
    }
}
{% endhighlight %}