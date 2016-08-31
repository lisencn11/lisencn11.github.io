---
layout: post
title: Leetcode Problem 16 Summary
date: 2016-03-24
categories: blog
tags: [study]

---

# 题目描述

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

>For example, given array S = {-1 2 1 -4}, and target = 1.

>The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

# 思路

对原array排序，第一个数为从0到n－2的遍历，每次遍历分别选择i+1为第二个元素，n为第三个元素，当当前加和大于target时我们需要将第三个元素左移，小于则反之。通过这种方法将时间复杂度从O(n^3)降到O(n^2)。

# Java代码

{% highlight java %}
public class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int ret = 0;

        if (nums.length < 3) {
            for (int i = 0; i < nums.length; i++) {
                ret += nums[i];
            }
            return ret;
        }


        ret = nums[0] + nums[1] + nums[2];
        for (int i = 0; i < nums.length - 2; i++) {
            // first value iteration
            int j = i + 1;
            int k = nums.length - 1;
            while (j < k) {
                // second & third value iteration
                int tmp1 = Math.abs(target - ret);
                int tmpSum = nums[i] + nums[j] + nums[k];
                int tmp2 = Math.abs(target - tmpSum);
                if (tmp1 == 0) {
                    return ret;
                }
                if (tmp1 > tmp2) {
                    ret = tmpSum;
                }
                if (tmpSum > target) {
                    k--;
                } else {
                    j++;
                }
            }
        }
        return ret;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int len = nums.length;
        if (len < 3) return 0;
        Arrays.sort(nums);
        int closest = nums[0] + nums[1] + nums[2];
        int minDiff = Integer.MAX_VALUE;
        for (int i = 0; i < len - 2; i++) {
            int p1 = i + 1;
            int p2 = len - 1;
            while (p1 < p2) {
                int sum = nums[i] + nums[p1] + nums[p2];
                int diff = Math.abs(sum - target);
                if (diff < minDiff) {
                    minDiff = diff;
                    closest = sum;
                }
                if (sum > target) p2--;
                else if (sum < target) p1++;
                else return sum;
            }
        }
        return closest;
    }
}
{% endhighlight %}