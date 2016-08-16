---
layout: post
title: Leetcode Problem 119 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,  
Return [1,3,3,1].

Note:  
Could you optimize your algorithm to use only O(k) extra space?

# 我的算法

与之前的118题解法相同，只是改成只用两个List即可。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> preRow = new ArrayList<>();
        List<Integer> currRow = null;
        
        preRow.add(1);
        for (int i = 1; i <= rowIndex; i++) {
            currRow = new ArrayList<>();
            currRow.add(1);
            for (int j = 0; j < preRow.size() - 1; j++) {
                currRow.add(preRow.get(j) + preRow.get(j + 1));
            }
            currRow.add(1);
            preRow = currRow;
        }
        
        return preRow;
    }
}
{% endhighlight %}