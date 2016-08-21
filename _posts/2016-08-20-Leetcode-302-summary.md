---
layout: post
title: Leetcode Problem 302 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

For example, given the following image:

[  
  "0010",  
  "0110",  
  "0100"  
]  
and x = 0, y = 2,  
Return 6.

# 我的算法

计算面积最关键的是得到四个值，最上最下最左最右，一开始我才用了广度优先搜索获得，通过但太慢了。

改用二分查找算法，因为给了一个坐标(x,y)那么我们需要，以搜索最左值为例：我们取 mid = (0 + x) / 2 对整列进行搜索看是否有 1，以此类推。

# 代码

### 广度优先搜索

{% highlight java %}
import java.awt.Point;
public class Solution {
    public int minArea(char[][] image, int x, int y) {
        int high = x, low = x, left = y, right = y;
        Set<Point> set = new HashSet<>();
        Queue<Point> queue = new LinkedList<>();
        Point point = new Point(x, y);
        queue.offer(point);
        set.add(point);
        
        while (!queue.isEmpty()) {
            point = queue.poll();
            int row = (int)point.getX();
            int col = (int)point.getY();
            high = row > high ? row : high;
            low = row < low ? row : low;
            right = col > right ? col : right;
            left = col < left ? col : left;
            Point[] points = new Point[4];
            points[0] = new Point(row - 1, col);
            points[1] = new Point(row, col - 1);
            points[2] = new Point(row + 1, col);
            points[3] = new Point(row, col + 1);
            for (int i = 0; i < 4; i++) {
                int tempX = (int)points[i].getX();
                int tempY = (int)points[i].getY();
                if (tempX < 0 || tempX >= image.length || tempY < 0 || tempY >= image[0].length) {
                    continue;
                }
                if (image[tempX][tempY] == '1' && !set.contains(points[i])) {
                    set.add(points[i]);
                    queue.offer(points[i]);
                }
            }
        }
        
        return (high - low + 1) * (right - left + 1);
    }
}
{% endhighlight %}

### 二分查找

{% highlight java %}
public class Solution {
    public int minArea(char[][] image, int x, int y) {
        int row = image.length;
        int col = image[0].length;
        int left = search(image, 0, y, row, true, false);
        int right = search(image, y, col, row, false, false);
        int top = search(image, 0, x, col, true, true);
        int bottom = search(image, x, row, col, false, true);
        return (right - left) * (bottom - top);
    }
    
    private int search(char[][] image, int low, int high, int rowOrCol, boolean leftOrTop, boolean row) {
        while (low < high) {
            boolean found = false;
            int mid = low + (high - low) / 2;
            for (int i = 0; i < rowOrCol; i++) {
                if ((!row && image[i][mid] == '1') || (row && image[mid][i] == '1')) {
                    if (leftOrTop) high = mid;
                    else low = mid + 1;
                    found = true;
                    break;
                }
            }
            if (!found) {
                if (leftOrTop) low = mid + 1;
                else high = mid;
            }
        }
        return low;
    }
}
{% endhighlight %}