---
layout: post
title: Leetcode Problem 338 Summary
date: 2016-04-20
categories: blog
tags: [study]

---

# 题目

输入是一个非负整数num，输出从0到num的每个数的二进制表示有多少个1，输出一个长度为num+1的int数组，其中存放着每个二进制数的1的个数。要求算法复杂度O(n)

# 算法

对每个数求1的个数然后求num+1次是最简单的方法，算法复杂度是O(n*sizeof(1+2+...+num+1))。要达到O(n)的算法复杂度，仔细思考有两种可能性：

* 对每个数求二进制有多少个1存在O(1)的算法
* 后面的二进制有多少个1能根据前面已经求出来的计算而得，也就是动态规划算法

第一种可能性基本排除，因为直觉感觉不可能，所以显然要用动态规划算法。得到递推公式：

	result[i] = result[i & (i - 1)] + 1
	
i & (i - 1)是bit manipulation常用手段，得到的是i去掉最后一位1的结果，这个结果比i小，所以之前必然已经计算得出结果，而这两个数相差1的个数则是1，所以加1得到i的二进制表示中所含1的个数。

# 代码

```java

class Solution {
	public int[] countBits(int num) {
		int[] result = new int[num + 1];
		result[0] = 0;
		for (int i = 1; i <= num; i++) {
			result[i] = result[i&(i-1)] + 1;
		}
		return result;
	}
}

```