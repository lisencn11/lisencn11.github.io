---
layout: post
title: Leetcode Problem 264 Summary
date: 2016-06-08
categories: blog
tags: [study]

---

# 题目

**丑数**是指因式分解后又2， 3， 5组成的数，如10=2*5。约定1是第1个丑数，设计算法求第n个丑数。


# 算法

比较粗暴的算法是从2开始挨个测试是否丑数，算法复杂度极低。经过观察发现丑数都有已有丑数乘上2，3，5产生，故可以使用动态规划存储已有k-1个丑数从而产生第k个丑数。难点在于递推关系式：通过分析，发现如果第k-1个丑数为*ugly[k-1]=min(ugly[i1] \* 2, ugly[i2] \* 3, ugly[i3] \* 5)*，且*ugly[k-1]==ugly[i1] \* 2*，则第k个丑数为*ugly[k-1]=min(ugly[i1 + 1] \* 2, ugly[i2] \* 3, ugly[i3] \* 5)*。

# 代码

```java

public class Solution {
    private int min(int i1, int i2, int i3) {
        int min;
        min = Math.min(i1, i2);
        min = Math.min(min, i3);
        return min;
    }
    
    public int nthUglyNumber(int n) {
        int[] res = new int[n];
        int i1 = 0, i2 = 0, i3 = 0;
        res[0] = 1;
        
        for (int i = 1; i < n; i++) {
            res[i] = min(res[i1] * 2, res[i2] * 3, res[i3] * 5);
            if (res[i] == (res[i1] * 2)) i1++;
            if (res[i] == (res[i2] * 3)) i2++;
            if (res[i] == (res[i3] * 5)) i3++;
        }
        
        return res[n - 1];
    }
}

```

