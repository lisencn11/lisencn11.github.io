---
layout: post
title: Leetcode Problem 39 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:  
* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.
* 
For example, given candidate set [2, 3, 6, 7] and target 7, 

A solution set is:   
[  
  [7],  
  [2, 2, 3]  
]

# 我的算法

backtrack ，需要注意的是不能有重复组合，所以加了 pre 变量。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        Arrays.sort(candidates);
        helper(ret, list, candidates, target, 0);
        return ret;
    }
    
    private void helper(List<List<Integer>> ret, List<Integer> list, int[] candidates, int target, int pre) {
        if (target == 0) {
            ret.add(new ArrayList<>(list));
            return;
        }
        if (target < 0) return;
        
        for (int i = pre; i < candidates.length && candidates[i] <= target; i++) {
            list.add(candidates[i]);
            helper(ret, list, candidates, target - candidates[i], i);
            list.remove(list.size() - 1);
        }
    }
}
{% endhighlight %}