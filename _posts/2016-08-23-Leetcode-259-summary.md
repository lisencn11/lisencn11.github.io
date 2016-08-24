---
layout: post
title: Leetcode Problem 259 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given an array of n integers nums and a target, find the number of index triplets i, j, k with 0 <= i < j < k < n that satisfy the condition nums[i] + nums[j] + nums[k] < target.

For example, given nums = [-2, 0, 1, 3], and target = 2.

Return 2. Because there are two triplets which sums are less than 2:

[-2, 0, 1]  
[-2, 0, 3]  
Follow up:  
Could you solve it in O(n2) runtime?

# 我的算法

固定第一个点后，用 two pointer 算法，对后两个点的搭配组合数进行求解，假设两点 index 为 j 和 k，我们对每个 j 只需要找到能够满足条件：nums[i] + nums[j] + nums[k] < target 的最大的 k 即可，因为若某个 k 满足条件，i 和 j 不变时，更小的 k 也必然满足条件。

# 代码

{% highlight java %}
public class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int count = 0;
        for (int i = 0; i < nums.length - 2; i++) {
            int j = i + 1, k = nums.length - 1;
            while (j < k) {
                if (nums[i] + nums[j] + nums[k] < target) {
                    count += k - j;
                    j++;
                } else {
                    k--;
                }
            }
        }
        return count;
    }
}
{% endhighlight %}