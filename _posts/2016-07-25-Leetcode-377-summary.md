---
layout: post
title: Leetcode Problem 377 Summary
date: 2016-07-25
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组，全部元素均为正，没有重复元素，一个 target目标值

**输出**数组中元素加和为目标值的可能组合数，可以重复使用元素，元素前后顺序不同，算作不同的组合。

如：

nums = [1, 2, 3]  
target = 4

The possible combination ways are:  
(1, 1, 1, 1)  
(1, 1, 2)  
(1, 2, 1)  
(1, 3)  
(2, 1, 1)  
(2, 2)  
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.

# 我的算法

动态规划算法，对于一个目标值 n ，其所有可能组合等于 n - nums[0] ， n - nums[1] ， ... 的组合数加和。

# 代码

{% highlight java %}
public class Solution {
    public int combinationSum4(int[] nums, int target) {
        if (nums == null || nums.length == 0) return 0;
        
        int[] comb = new int[target + 1];
        comb[0] = 1;
        Arrays.sort(nums);
        for (int i = 1; i <= target; i++) {
            for (int j = 0; j < nums.length && i - nums[j] >= 0; j++) {
                comb[i] += comb[i - nums[j]];
            }
        }
        
        return comb[target];
    }
}
{% endhighlight %}