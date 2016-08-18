---
layout: post
title: Leetcode Problem 6 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

![problem6](https://lisencn11.github.io/img/problem6.png)

And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string text, int nRows);

convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

# 我的算法

因为会调用 sumRange() 很多次，所以考虑动态规划算法，将从 0 到 j 的和保存起来， 求从 i 到 j 的和只需要用 0 到 j 的和减去 0 到 i - 1 的和即可。

# 代码

{% highlight java %}
public class NumArray {
    int[] sums;

    public NumArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            sums = nums;
            return;
        }
        
        sums = new int[nums.length];
        sums[0] = nums[0];
        for (int i = 1; i < nums.length; i++)
            sums[i] = nums[i] + sums[i - 1];
    }

    public int sumRange(int i, int j) {
        if (sums == null || sums.length == 0) return 0;
        
        if (i == 0) return sums[j];
        else return sums[j] - sums[i - 1];
    }
}


// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);
{% endhighlight %}