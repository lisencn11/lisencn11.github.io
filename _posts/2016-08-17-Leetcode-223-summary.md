---
layout: post
title: Leetcode Problem 223 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

![](https://lisencn11.github.io/img/problem223.png)

Assume that the total area is never beyond the maximum possible value of int.

# 我的算法

两个单独面积算出后，判断是否有重叠，没有直接相加，有的话还需要减去重叠部分面积。

# 代码

{% highlight java %}
public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int area1 = (C - A) * (D - B);
        int area2 = (G - E) * (H - F);
        int[] height = new int[4];
        int[] width = new int[4];
        if (E > C || G < A || F > D || H < B) return area1 + area2;
        height[0] = A;
        height[1] = C;
        height[2] = E;
        height[3] = G;
        width[0] = B;
        width[1] = D;
        width[2] = F;
        width[3] = H;
        Arrays.sort(height);
        Arrays.sort(width);
        int area = (height[2] - height[1]) * (width[2] - width[1]);
        return area1 + area2 - area;
    }
}
{% endhighlight %}

# 更好的方案

{% highlight java %}
int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
    int left = max(A,E), right = max(min(C,G), left);
    int bottom = max(B,F), top = max(min(D,H), bottom);
    return (C-A)*(D-B) - (right-left)*(top-bottom) + (G-E)*(H-F);
}
{% endhighlight %}