---
layout: post
title: Leetcode Problem 42 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example,   
Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

![](https://lisencn11.github.io/img/problem42.png)

# 我的算法

two pointers 一个 left 一个 right，分别从两边向中间，最高的那一根不动，次高的那一根移动并不断计算，这道题不太好解释，看[这里](http://www.xuebuyuan.com/1586534.html)

# 代码

{% highlight java %}
public class Solution {
    public int trap(int[] height) {
        int second = 0;
        int left = 0;
        int right = height.length - 1;
        int area = 0;
        
        while (left < right) {
            if (height[left] < height[right]) {
                second = height[left] > second ? height[left] : second;
                area += second - height[left];
                left++;
            } else {
                second = height[right] > second ? height[right] : second;
                area += second - height[right];
                right--;
            }
        }
        return area;
    }
}
{% endhighlight %}