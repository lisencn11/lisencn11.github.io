---
layout: post
title: Leetcode Problem 118 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,  
Return

[  
     [1],  
    [1,1],  
   [1,2,1],  
  [1,3,3,1],  
 [1,4,6,4,1]  
]

# 我的算法

纯粹ArrayList操作，没考察算法。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> row = new ArrayList<>();
        List<Integer> preRow = null;
        row.add(1);
        ret.add(row);
        
        if (numRows == 0) return new ArrayList<>();
        if (numRows == 1) return ret;
        
        for (int i = 2; i <= numRows; i++) {
            preRow = ret.get(ret.size() - 1);
            row = new ArrayList<>();
            row.add(1);
            for (int j = 0; j < preRow.size() - 1; j++) {
                row.add(preRow.get(j) + preRow.get(j + 1));
            }
            row.add(1);
            ret.add(row);
        }
        
        return ret;
    }
}
{% endhighlight %}