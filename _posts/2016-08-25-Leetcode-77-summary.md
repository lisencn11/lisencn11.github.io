---
layout: post
title: Leetcode Problem 77 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,  
If n = 4 and k = 2, a solution is:

[  
  [2,4],  
  [3,4],  
  [2,3],  
  [1,2],  
  [1,3],  
  [1,4],  
]

# 我的算法

backtrack ，递推关系 combine(n, k) = combine(n - 1, k - 1) + combine(n - 1, k) ，表示从 n 中选 k 的结果等于 n - 1 选 k - 1 加上 n 中选 k - 1 的结果。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
        if (k == 1) {
            List<List<Integer>> ret = new ArrayList<>();
            for (int i = 1; i <= n; i++) {
                List<Integer> list = new ArrayList<>();
                list.add(i);
                ret.add(list);
            }
            return ret;
        }
        if (n == k) {
            List<List<Integer>> ret = new ArrayList<>();
            List<Integer> list = new ArrayList<>();
            for (int i = 1; i <= n; i++) {
                list.add(i);
            }
            ret.add(list);
            return ret;
        }
        
        List<List<Integer>> subList1 = combine(n - 1, k - 1);
        List<List<Integer>> subList2 = combine(n - 1, k);
        
        for (int i = 0; i < subList1.size(); i++) {
            subList1.get(i).add(n);
        }
        
        subList1.addAll(subList2);
        return subList1;
    }
}
{% endhighlight %}

# discuss

相同思路，更简洁的代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
        if (k == n || k == 0) {
            List<Integer> row = new LinkedList<>();
            for (int i = 1; i <= k; ++i) {
                row.add(i);
            }
            return new LinkedList<>(Arrays.asList(row));
        }
        List<List<Integer>> result = this.combine(n - 1, k - 1);
        result.forEach(e -> e.add(n));
        result.addAll(this.combine(n - 1, k));
        return result;
    }
}
{% endhighlight %}