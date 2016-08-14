---
layout: post
title: Leetcode Problem 217 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

# 我的算法

简单应用HashSet，结果Time Limit Exceed，应该是测例的问题，O(n)的算法肯定是最快的了。用先排序再check相邻元素的方法反而通过了，但是这个算法是O(nlogn)的复杂度啊。。。

# 代码

{% highlight java %}
public class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
		 for(int i : nums)
			 if(!set.add(i))
				 return true; 
		 return false;
    }
}
{% endhighlight %}