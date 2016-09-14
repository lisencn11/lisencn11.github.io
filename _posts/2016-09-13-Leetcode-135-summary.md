---
layout: post
title: Leetcode Problem 135 Summary
date: 2016-09-13
categories: blog
tags: [study]

---

# 题目

There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.
* 
What is the minimum candies you must give?

# 我的算法

假设对于下面这个例子 ： 9 5 5 6 3 8 9 6 5 2 。在 3 8 9 这三个 rating 的孩子我们应该分别给 1 2 4 个 candy。这个是这道题的核心问题。

一开始我的思路是，对于一个数看它左右的值，进行更新，当遇到不能更新的时候进行反馈修改，如到 5 时发现 candy数对应为 6 是 2 而 2 是 1，则 5 需要给 2 个 candy，反向对 6 和 9 就行更新。

但是这种方法对于一个极长的单调递减序列需要不断的更新，我想对于这种情况是不是从右到左扫描比较好，突然又一个猜想冒出，是否能先从左到右再从右到左遍历两遍，事实证明这种算法可行。

# 代码

{% highlight java %}
public class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
        int sum = 0;
        Arrays.fill(candies, 1);
        
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = Math.max(candies[i + 1] + 1, candies[i]);
            }
            sum += candies[i];
        }
        sum += candies[n - 1];
        return sum;
    }
}
{% endhighlight %}