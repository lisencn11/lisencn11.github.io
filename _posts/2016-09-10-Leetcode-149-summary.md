---
layout: post
title: Leetcode Problem 149 Summary
date: 2016-09-10
categories: blog
tags: [study]

---

# 题目

Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

# 我的算法

对点 a ，计算其和其他点的斜率，斜率相同则在同一条线上，用 Hash Table 保存斜率和点数对 map。

# 代码

{% highlight java %}
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    public int maxPoints(Point[] points) {
        int pointsLen = points.length;
        int max = 0;
        for (int i = 0; i < pointsLen; i++) {
            Map<String, Integer> map = new HashMap<>();
            int overlap = 0;
            int currMax = 0;
            for (int j = i + 1; j < pointsLen; j++) {
                int x = points[j].x - points[i].x;
                int y = points[j].y - points[i].y;
                if (x == 0 && y == 0) {
                    overlap++;
                    continue;
                }
                int gcd = gcd(x, y);
                x /= gcd;
                y /= gcd;
                
                String key = x + "," + y;
                if (!map.containsKey(key)) map.put(key, 0);
                int count = map.get(key);
                count++;
                map.put(key, count);
                currMax = Math.max(count, currMax);
            }
            currMax += overlap + 1;
            max = Math.max(currMax, max);
        }
        return max;
    }
    
    private int gcd(int a, int b) {
        if (b == 0) return a;
        else return gcd(b, a % b);
    }
}
{% endhighlight %}