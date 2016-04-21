---
layout: post
title: Leetcode Problem 342 Summary
date: 2016-04-18
categories: blog
tags: [study]

---

# 题目

在不使用循环语句的情况下如何判断一个数是4的n次方。

# 算法

首先想到是进行位操作，因为不能使用循环语句。4的n次方的特点是这个数同时也是2的2n次方，即0x55555555，所以需要num&0x55555555不为0。另外一个就是4的n次方的表示在32位数中只有一位是1，用num&(num-1)==0可以判断是否32位中只有一位是1。

# 代码

```java

public class Solution {
	public boolean isPowerOfFour(int num) {
    	return num > 0 && (num&(num-1)) == 0 && ((num&0x55555555) != 0);
	}
}

```