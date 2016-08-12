---
layout: post
title: Leetcode Problem 292 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

Hint:

If there are 5 stones in the heap, could you figure out a way to remove the stones such that you will always be the winner?

# 我的算法

观察发现若有一二三个石头直接赢，四个石头必输，那么五六七个石头可以使对手面临四个石头必输的情况，得出结论石头数不能是四的倍数。

# 代码

{% highlight java %}
public class Solution {
    public boolean canWinNim(int n) {
        return !(n % 4 == 0) || (n == 0);
    }
}
{% endhighlight %}