---
layout: post
title: Leetcode Problem 313 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k. For example, [1, 2, 4, 7, 8, 13, 14, 16, 19, 26, 28, 32] is the sequence of the first 12 super ugly numbers given primes = [2, 7, 13, 19] of size 4.

Note:  
(1) 1 is a super ugly number for any given primes.  
(2) The given numbers in primes are in ascending order.  
(3) 0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000.

# 我的算法

基本思路：

* 所有正整数都可以由质数相乘而得
* 第 i 个质数必定由第 i 个质数之前的某个质数乘以 primes 数组中的某个而得。

那么最开始我的想法是：从 1 开始，乘以数组中每个元素，扔到一个最小堆中，然后从最小堆中不断取出元素，乘以数组每个元素，知道第 n 个元素。

这个算法的复杂度是 O(mnlogmn) m 是数组长度，n是所求的第 n 个数。

起始更简单的是维护两个数组，int[] ugly 记录第 i 个 ugly number， int[] idx 记录数组中第 i 个 prime number当前乘到了第几个 ugly number。算法复杂度 O(mn)

# 代码

{% highlight java %}
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int[] ugly = new int[n];
        int[] idx = new int[primes.length];
        
        ugly[0] = 1;
        for (int i = 1; i < n; i++) {
            ugly[i] = Integer.MAX_VALUE;
            for (int j = 0; j < primes.length; j++) {
                int currUgly = primes[j] * ugly[idx[j]];
                ugly[i] = currUgly < ugly[i] ? currUgly : ugly[i];
            }
            
            for (int j = 0; j < primes.length; j++) {
                if (primes[j] * ugly[idx[j]] <= ugly[i]) idx[j]++;
            }
        }
        return ugly[n - 1];
    }
}
{% endhighlight %}

### 继续优化

避免重复的乘法运算和多余的循环

{% highlight java %}
public int nthSuperUglyNumber(int n, int[] primes) {
    int[] ugly = new int[n];
    int[] idx = new int[primes.length];
    int[] val = new int[primes.length];
    Arrays.fill(val, 1);

    int next = 1;
    for (int i = 0; i < n; i++) {
        ugly[i] = next;
        
        next = Integer.MAX_VALUE;
        for (int j = 0; j < primes.length; j++) {
            //skip duplicate and avoid extra multiplication
            if (val[j] == ugly[i]) val[j] = ugly[idx[j]++] * primes[j];
            //find next ugly number
            next = Math.min(next, val[j]);
        }
    }

    return ugly[n - 1];
}
{% endhighlight %}

### 最终优化

截至目前，我们需要 O(m) 的复杂度选出当前最小的候选数，使用最小堆可以实现 O(logm) 的复杂度，也就是总体 O(nlogm) 的复杂度。

{% highlight java %}
public int nthSuperUglyNumberHeap(int n, int[] primes) {
    int[] ugly = new int[n];

    PriorityQueue<Num> pq = new PriorityQueue<>();
    for (int i = 0; i < primes.length; i++) pq.add(new Num(primes[i], 1, primes[i]));
    ugly[0] = 1;

    for (int i = 1; i < n; i++) {
        ugly[i] = pq.peek().val;
        while (pq.peek().val == ugly[i]) {
            Num nxt = pq.poll();
            pq.add(new Num(nxt.p * ugly[nxt.idx], nxt.idx + 1, nxt.p));
        }
    }

    return ugly[n - 1];
}

private class Num implements Comparable<Num> {
    int val;
    int idx;
    int p;

    public Num(int val, int idx, int p) {
        this.val = val;
        this.idx = idx;
        this.p = p;
    }

    @Override
    public int compareTo(Num that) {
        return this.val - that.val;
    }
}
{% endhighlight %}