---
layout: post
title: Leetcode Problem 84 Summary
date: 2016-09-11
categories: blog
tags: [study]

---

# 题目

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![](https://lisencn11.github.io/img/problem84.png)

For example,  
Given heights = [2,1,5,6,2,3],  
return 10.

# 我的算法

使用一个 Stack ，Stack 中保存着至今为止的升序高度的 index：

1. 若遍历到的 histogram 高于 stack 顶的高度，push 进去
2. 若遍历到的 histogram 低于 stack 顶的高度， pop 出来
3. 对 pop 出来的 index 求面积，宽等于 i - stack.peek() - 1 ，i 表示当前遇到的高度小于它的 index，即 pop 出的高度所能平移到的最右端 (exclusive)， stack.peek() 表示所能平移到的最左端。

# 代码

{% highlight java %}
public class Solution {
    public int largestRectangleArea(int[] heights) {
        if (heights == null || heights.length == 0) return 0;
        Stack<Integer> stack = new Stack<>();
        int max = 0;
        for (int i = 0; i < heights.length; i++) {
            if (stack.isEmpty() || heights[i] >= heights[stack.peek()]) {
                stack.push(i);
            } else {
                while (!stack.isEmpty() && heights[i] <= heights[stack.peek()]) {
                    int index = stack.pop();
                    int h = heights[index];
                    int w = 0;
                    if (stack.isEmpty()) {
                        w = i;
                    } else {
                        w = i - stack.peek() - 1;
                    }
                    max = Math.max(h * w, max);
                }
                stack.push(i);
            }
        }
        while (!stack.isEmpty()) {
            int index = stack.pop();
            int h = heights[index];
            int w = 0;
            if (stack.isEmpty()) {
                w = heights.length;
            } else {
                w = heights.length - stack.peek() - 1;
            }
            max = Math.max(h * w, max);
        }
        return max;
    }
}
{% endhighlight %}