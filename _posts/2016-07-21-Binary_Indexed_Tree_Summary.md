---
layout: post
title: Binary Indexed Tree Summary
date: 2016-07-21
categories: blog
tags: [study]

---

# 树状数组

树状数组可用于**动态区间求和**，与线段树一样可以达到**修改**和**查询**同时为O(logn)的时间复杂度。

# 原理

![binary indexed tree](http://images2015.cnblogs.com/blog/675542/201509/675542-20150914140031867-399947421.png)

其中 A 数组是我们**输入的**数组， C 数组是**和**数组，于是我们有：  
C1 = A1  
C2 = A1 + A2  
C3 = A3  
C4 = A1 + A2 + A3 + A4  
C5 = A5  
C6 = A5 + A6  
C7 = A7  
C8 = A1 + A2 + A3 + A4 + A5 + A6 + A7 + A8  
求解任何的和都可以通过几个 C 数组元素加和求得，如 sum[7] = C[4] + C[6] + C[7]。

# 性质

现在假设我们有这么一个树状数组，给出一个 index i ，问题就变成了如何求得 sum[i] 由哪几个节点相加而成。

**一个有趣的性质**：对于编号为 x 的节点，这个节点的管辖区间为 2<sup>k</sup> ，其中 k 为 x 二进制表示中末尾 0 的个数。如： C[4] 的管辖区间为 4 ，C[4] 是 A[1] + A[2] + A[3] + A[4] 。所以我们得到 C[n] = A[n - 2<sup>k</sup> + 1] + ... + A[n]，这里面 k 的值可以通过下面这个方法求得。

{% highlight java %}
int lowbit(int x) {
    return x & (-x);
}
{% endhighlight %}

解释：-x 是 x 的补码，即反码加 1 。假设 x 尾部有 n 个 0 ，求反后将变为 1 ，加 1 后会得到 从右向左第 n+1 位为 1 ，而得到的数也恰好是 2<sup>k</sup> 。

# 算法

### 求和

1. 设 sum = 0
2. 若 n <= 0 ，算法结束，返回 sum 值。否则 sum = sum + C[n]
3. n = n - lowbit(n) ，返回第 2 步

### 修改

1. 若 i > n ，算法结束
2. C[i] = C[i] + diff ， i = i + lowbit(i) ，转第 1 步