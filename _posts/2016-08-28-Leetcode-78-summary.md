---
layout: post
title: Leetcode Problem 78 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,  
If nums = [1,2,3], a solution is:

[  
  [3],  
  [1],  
  [2],  
  [1,2,3],  
  [1,3],  
  [2,3],  
  [1,2],  
  []  
]

# 我的算法

backtrack

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        helper(ret, list, 0, nums);
        return ret;
    }
    
    private void helper(List<List<Integer>> ret, List<Integer> list, int start, int[] nums) {
        if (start >= nums.length) {
            ret.add(new ArrayList<>(list));
            return;
        }
        
        helper(ret, list, nums.length, nums);
        for (int i = start; i < nums.length; i++) {
            list.add(nums[i]);
            helper(ret, list, i + 1, nums);
            list.remove(list.size() - 1);
        }
    }
}
{% endhighlight %}