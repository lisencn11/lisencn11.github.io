---
layout: post
title: Leetcode Problem 391 Summary
date: 2016-09-08
categories: blog
tags: [study]

---

# 题目

Given N axis-aligned rectangles where N > 0, determine if they all together form an exact cover of a rectangular region.

Each rectangle is represented as a bottom-left point and a top-right point. For example, a unit square is represented as [1,1,2,2]. (coordinate of bottom-left point is (1, 1) and top-right point is (2, 2)).

![](https://lisencn11.github.io/img/problem391.png)

# 我的算法

一个有效的解应该满足下面两个条件：

1. 分为三种点：一是四个角，只有一个矩形相接；二是 T 字型的点，有两个矩形相接；三是 十字型 的点，有四个矩形相接；
2. 同一个点，可能由左上，左下，右上，右下四个矩形相接，但是每个方向只能有一个矩阵存在

所以我们：

1. 遍历各点，对每个点存是否存在左上，左下，右上，右下四个矩形，出现重复矩阵就返回 false
2. 遍历存储的点集，如果有点存在不符合要求的矩形组合，比如左上和右下，返回false

# 代码

{% highlight java %}
public class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        int leftUpper = 1, rightUpper = 2, leftLower = 4, rightLower = 8;
        Map<String, Integer> map = new HashMap<>();
        int minX = Integer.MAX_VALUE, maxX = Integer.MIN_VALUE, minY = Integer.MAX_VALUE, maxY = Integer.MIN_VALUE;
        for (int[] rect : rectangles) {
            int leftX = rect[0];
            int leftY = rect[1];
            int rightX = rect[2];
            int rightY = rect[3];
            minX = leftX < minX ? leftX : minX;
            minY = leftY < minY ? leftY : minY;
            maxX = rightX > maxX ? rightX : maxX;
            maxY = rightY > maxY ? rightY : maxY;
            if (!insertNode(leftX, leftY, leftLower, map)) return false;
            if (!insertNode(leftX, rightY, leftUpper, map)) return false;
            if (!insertNode(rightX, leftY, rightLower, map)) return false;
            if (!insertNode(rightX, rightY, rightUpper, map)) return false;
        }
        
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            int value = entry.getValue();
            String[] temp = entry.getKey().split(",");
            int x = Integer.parseInt(temp[0]);
            int y = Integer.parseInt(temp[1]);
            if ((x == minX || x == maxX) && (y == minY || y == maxY)) {
                if (!(value == leftUpper || value == rightUpper || value == leftLower || value == rightLower)) {
                    // System.out.println("1" + " " + x + " " + y + " " + value);
                    return false;
                }
            } else {
                if (!(value == 3 || value == 5 || value == 10 || value == 12 || value == 15)) {
                    // System.out.println("2" + " " + x + " " + y + " " + value);
                    return false;
                }
            }
        }
        return true;
    }
    
    private boolean insertNode(int x, int y, int cornerBit, Map<String, Integer> map) {
        String key = new String(x + "," + y);
        if (!map.containsKey(key)) map.put(key, 0);
        int value = map.get(key);
        if ((value & cornerBit) == 1) return false;
        else {
            value = (value | cornerBit);
            map.put(key, value);
            return true;
        }
    }
}
{% endhighlight %}