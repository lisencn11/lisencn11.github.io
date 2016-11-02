---
layout: post
title: Leetcode Problem 413 Summary
date: 2016-10-15
categories: blog
tags: [study]

---

# 题目

A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9

The following sequence is not arithmetic.

1, 1, 2, 5, 7

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:  
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.


Example:

A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.

# 我的算法

纪录当前起点和差值即可。

# 代码

{% highlight java %}
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        if (A == null || A.length < 3) return 0;
        
        int diff = A[1] - A[0];
        int start = 0;
        int result = 0;
        
        for (int i = 2; i < A.length; i++) {
            int currDiff = A[i] - A[i - 1];
            if (currDiff == diff) {
                result += i - start - 1;
            } else {
                diff = currDiff;
                start = i - 1;
            }
        }
        
        return result;
    }
}
{% endhighlight %}