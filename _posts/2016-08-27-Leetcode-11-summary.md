---
layout: post
title: Leetcode Problem 11 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.

# 我的算法

首先注意题意的理解，刚开始我以为是有多个 container，找出能装水最多的那一个，然而实际是有多个 container 边界，找出两个边界组成装水最多的 container，也就是说这些边界刚开始是没有的，最后留下的是选中的两个，而非我理解的这些边界一开始就有，选择两个盛水最多的。

two pointer 算法，画图发现，如果当前我们选中的是 a<sub>i</sub> 和 a<sub>j</sub> 两个，假定 a<sub>i</sub> < a<sub>j</sub> ，那么我们发现对 a<sub>k</sub>，k > i ，a<sub>k</sub> 和 a<sub>j</sub> 组成的 container 只有在 k 的高度大于 i 的时候才有可能更大。

所以我们对两个 pointer 选出较小的那一个，向另一个移动直到找出比移动前更大的高度为止。

# 代码

{% highlight java %}
public class Solution {
    public int maxArea(int[] height) {
        int n = height.length;
        int p1 = 0;
        int p2 = n - 1;
        int max = 0;
        
        while (p1 < p2) {
            int vol = (p2 - p1) * Math.min(height[p1], height[p2]);
            max = vol > max ? vol : max;
            if (height[p1] < height[p2]) {
                int mark = height[p1];
                while (p1 < p2 && height[p1] <= mark) p1++;
            } else {
                int mark = height[p2];
                while (p1 < p2 && height[p2] <= mark) p2--;
            }
        }
        
        return max;
    }
}
{% endhighlight %}