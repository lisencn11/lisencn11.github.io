---
layout: post
title: Leetcode Problem 216 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.


Example 1:

Input: k = 3, n = 7

Output:

[[1,2,4]]

Example 2:

Input: k = 3, n = 9

Output:

[[1,2,6], [1,3,5], [2,3,4]]

# 我的算法

backtrack

# 代码

{% highlight java %}
public class Solution {
    static final int LOWER_BOUND = 1;
    static final int HIGHER_BOUND = 9;
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        helper(ret, list, k, n, LOWER_BOUND);
        return ret;
    }
    
    private void helper(List<List<Integer>> ret, List<Integer> list, int k, int n, int start) {
        if ((k == 0 && n != 0) || (k != 0 && n == 0)) return;
        if (k < 0 || n < 0) return;
        if (k == 0 && n == 0) {
            List<Integer> result = new ArrayList<>(list);
            ret.add(result);
        }
        
        for (int i = start; i <= HIGHER_BOUND; i++) {
            list.add(i);
            helper(ret, list, k - 1, n - i, i + 1);
            list.remove(list.size() - 1);
        }
    }
}
{% endhighlight %}