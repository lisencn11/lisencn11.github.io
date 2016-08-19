---
layout: post
title: Leetcode Problem 296 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.

For example, given three people living at (0,0), (0,4), and (2,2):

![problem296](https://lisencn11.github.io/img/problem296.png)

The point (0,2) is an ideal meeting point, as the total travel distance of 2+2+2=6 is minimal. So return 6.

Hint:

Try to solve it in one dimension first. How can this solution apply to the two dimension case?

# 我的算法

从最简单的情况开始，如果是 1D 的情况怎么办？

我的想法是取均值能使总距离最短，但是事实证明这个想法是错的。

正确的方法是对线上的所有点从小到大排序，对于第一个和最后一个这两个点，取他们中间的任意点并不会影响他们的总距离。所以我们得到结论，对于直线上的 n 个点排序后，两个pointer分别从前向后和从后向前一对一对扫描，最小距离就是两点距离，直到同时到达某个点（总共奇数个点）或者相邻为止。

# 代码

{% highlight java %}
public class Solution {
    public int minTotalDistance(int[][] grid) {
        List<Integer> row = new ArrayList<>();
        List<Integer> col = new ArrayList<>();
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] != 0) {
                    row.add(i);
                    col.add(j);
                }
            }
        }
        
        return getMin(row) + getMin(col);
    }
    
    private int getMin(List<Integer> list) {
        int sum = 0;
        Collections.sort(list);
        
        int head = 0;
        int tail = list.size() - 1;
        
        while (head < tail) {
            sum += list.get(tail--) - list.get(head++);
        }
        
        return sum;
    }
}
{% endhighlight %}