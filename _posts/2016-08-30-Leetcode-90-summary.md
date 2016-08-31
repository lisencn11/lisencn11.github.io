---
layout: post
title: Leetcode Problem 260 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Given a collection of integers that might contain duplicates, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,  
If nums = [1,2,2], a solution is:

[  
  [2],  
  [1],  
  [1,2,2],  
  [2,2],  
  [1,2],  
  []  
]

# 我的算法

backtrack ，注意两点：

1. 由于包含重复元素，而不能包含重复子集，对于元素 i 如果不选，则后面所有相同的 i 都不选，以避免重复子集
2. 集合无序，需要提前排序。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        getSubsets(ret, list, nums, 0);
        return ret;
    }
    
    private void getSubsets(List<List<Integer>> ret, List<Integer> list, int[] nums, int start) {
        if (start >= nums.length) {
            ret.add(new ArrayList<>(list));
            return;
        }
        int next = start;
        while (next < nums.length - 1 && nums[next] == nums[next + 1]) next++;
        getSubsets(ret, list, nums, next + 1);
        list.add(nums[start]);
        getSubsets(ret, list, nums, start + 1);
        list.remove(list.size() - 1);
    }
}
{% endhighlight %}