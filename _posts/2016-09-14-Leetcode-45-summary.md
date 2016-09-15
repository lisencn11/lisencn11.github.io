---
layout: post
title: Leetcode Problem 45 Summary
date: 2016-09-14
categories: blog
tags: [study]

---

# 题目

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

For example:  
Given array A = [2,3,1,1,4]

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

Note:  
You can assume that you can always reach the last index.

# 我的算法

贪心算法，记录当前能够达到的最远 currMax ， 每一步是一个范围，当前步最远 finalMax。

# 代码

{% highlight java %}
public class Solution {
    public int jump(int[] nums) {
        int currMax = 0, finalMax = 0, count = 0;
        
        for (int i = 0; i < nums.length - 1; i++) {
            currMax = Math.max(currMax, i + nums[i]);
            if (i == finalMax) {
                count++;
                finalMax = currMax;
            }
        }
        return count;
    }
}
{% endhighlight %}