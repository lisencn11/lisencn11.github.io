---
layout: post
title: Leetcode Problem 1 Summary
date: 2016-04-19
categories: blog
tags: [study]

---

# 题目

给一个int array和一个target值，数组中有两个数加和为target，返回这两个数的下标。

# 算法

最开始的想法是用另一个数组储存排序后的原数组，排序后的数组从两头找，比target大就high--，比target小就low++，找到后再在原数组中找这两个数的下标。这种方法可以，但之后还需处理边界条件，如target由两个相同的数组成，则需要处理边界条件。

相比较而言是用HashMap的方法更加简单，不用处理边界条件，key为nums[i]，value为index，只需要查看HashMap是否含有target-nums[i]，如果有，说明找到两个数。

# 代码

```java

public class Solution {
  	public int[] twoSum(int[] nums, int target) {
  	int[] result = new int[2];
      	Map<Integer, Integer> map = new HashMap<Integer, Integer>();
      	for (int i = 0; i < nums.length; i++) {
      		if (map.containsKey(new Integer(target - nums[i]))) {
             		result[1] = i;
             		result[0] = map.get(new Integer(target - nums[i]));
             		break;
         		} else {
              	map.put(new Integer(nums[i]), new Integer(i));
          	}
      	}
      	return result;
  	}
}

```