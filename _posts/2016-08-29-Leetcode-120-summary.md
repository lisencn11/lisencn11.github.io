---
layout: post
title: Leetcode Problem 120 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle  
[  
     [2],  
    [3,4],  
   [6,5,7],  
  [4,1,8,3]  
]  
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

Note:  
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

# 我的算法

记录前一行的最小值，当前行的最小值可由前一行的最小值求得。

# 代码

{% highlight java %}
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle == null || triangle.size() == 0) return 0;
        List<Integer> pre = new ArrayList<>();
        pre.addAll(triangle.get(0));
        for (int i = 1; i < triangle.size(); i++) {
            List<Integer> list = triangle.get(i);
            List<Integer> curr = new ArrayList<>();
            for (int j = 0; j < list.size(); j++) {
                if (j == 0) curr.add(pre.get(j) + list.get(j));
                else if (j == list.size() - 1) curr.add(pre.get(j - 1) + list.get(j));
                else {
                    int min = Math.min(pre.get(j - 1), pre.get(j)) + list.get(j);
                    curr.add(min);
                }
            }
            pre = curr;
        }
        
        int min = Integer.MAX_VALUE;
        for (Integer i : pre) min = i < min ? i : min;
        return min;
    }
}
{% endhighlight %}