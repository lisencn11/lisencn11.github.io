---
layout: post
title: Leetcode Problem 330 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Given a sorted positive integer array nums and an integer n, add/patch elements to the array such that any number in range [1, n] inclusive can be formed by the sum of some elements in the array. Return the minimum number of patches required.

Example 1:  
nums = [1, 3], n = 6  
Return 1.

Combinations of nums are [1], [3], [1,3], which form possible sums of: 1, 3, 4.  
Now if we add/patch 2 to nums, the combinations are: [1], [2], [3], [1,3], [2,3], [1,2,3].  
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range [1, 6].  
So we only need 1 patch.

Example 2:  
nums = [1, 5, 10], n = 20  
Return 2.  
The two patches can be [2, 4].

Example 3:  
nums = [1, 2, 2], n = 5  
Return 0.

# 我的算法

一开始的做法是生成一个combination，然后选择不在其中的最小元素加进去，超时。

discuss 的做法在[这里](https://discuss.leetcode.com/topic/35494/solution-explanation)。基本思想事：选择目前遍历到 i 个元素最小的不能产生的加和为 miss ，表示我们能够通过前 i - 1 个元素的组合，表示所有 [0, miss) 的数，那么如果 num[i] <= miss，那么我们就又能表示 [num[i], num[i] + miss) 而 num[i] <= miss ，所以我们能够表示 [0, num[i] + miss) 这个范围的数。

# 代码

{% highlight java %}
public class Solution {
    public int minPatches(int[] nums, int n) {
        long miss = 1, count = 0, i = 0;
        while (miss <= n) {
            if (i < nums.length && nums[(int) i] <= miss) {
                miss += nums[(int) i++];
            } else {
                miss += miss;
                count++;
            }
        }
        return (int) count;
    }
}
{% endhighlight %}