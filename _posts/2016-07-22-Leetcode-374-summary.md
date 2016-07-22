---
layout: post
title: Leetcode Problem 374 Summary
date: 2016-07-22
categories: blog
tags: [study]

---

# 题目

**输入**一个整型 n ，在 1 到 n 的范围内猜一个数，提供一个 guess(int i) 的方法，猜的过大返回 -1 ，猜的过小返回 1 ，猜中返回 0 。

# 我的算法

二分查找即可。

# 代码

{% highlight java %}
/* The guess API is defined in the parent class GuessGame.
   @param num, your guess
   @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
      int guess(int num); */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int low = 1;
        int high = n;
        int mid = low + (high - low) / 2;
        int guess = guess(mid);

        while (guess(mid) != 0) {
            if (guess(mid) == 1) low = mid + 1;
            else high = mid - 1;
            mid = low + (high - low) / 2;
            guess = guess(mid);
        }
        
        return mid;
    }
}
{% endhighlight %}