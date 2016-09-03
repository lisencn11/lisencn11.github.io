---
layout: post
title: Leetcode Problem 356 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

Example 1:  
Given points = [[1,1],[-1,1]], return true.

Example 2:  
Given points = [[1,1],[-1,-1]], return false.

Follow up:  
Could you do better than O(n2)?

Hint:

* Find the smallest and largest x-value for all points.
* If there is a line then it should be at y = (minX + maxX) / 2.
* For each point, make sure that it has a reflected point in the opposite side.

# 我的算法

1. 找到最大最小元素
2. 计算出可能的对称轴
3. check 是否每一个点在对称轴另一边都有一个点对称点

# 代码

{% highlight java %}
public class Solution {
    public boolean isReflected(int[][] points) {
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        Set<String> set = new HashSet<>();
        for (int[] point : points) {
            min = point[0] < min ? point[0] : min;
            max = point[0] > max ? point[0] : max;
            set.add(point[0] + "a" + point[1]);
        }
        
        int sum = min + max;
        for (int[] point : points) {
            if (!set.contains((sum - point[0]) + "a" + point[1])) return false;
        }
        return true;
    }
}
{% endhighlight %}