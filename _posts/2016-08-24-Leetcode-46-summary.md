---
layout: post
title: Leetcode Problem 46 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Given a collection of distinct numbers, return all possible permutations.

For example,  
[1,2,3] have the following permutations:  
[  
  [1,2,3],  
  [1,3,2],  
  [2,1,3],  
  [2,3,1],  
  [3,1,2],  
  [3,2,1]  
]

# 我的算法

简单的 backtracking。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ret = new ArrayList<>();
        boolean[] vis = new boolean[nums.length];
        for (int i = 0; i < vis.length; i++) vis[i] = false;
        permutation(ret, new ArrayList<>(), 0, vis, nums);
        
        return ret;
    }
    
    private void permutation(List<List<Integer>> ret, List<Integer> list, int count, boolean[] vis, int[] nums) {
        if (count == nums.length) {
            ret.add(new ArrayList<>(list));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (vis[i]) continue;
            list.add(nums[i]);
            vis[i] = true;
            count++;
            permutation(ret, list, count, vis, nums);
            vis[i]  = false;
            count--;
            list.remove(list.size() - 1);
        }
    }
}
{% endhighlight %}