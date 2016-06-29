---
layout: post
title: Leetcode Problem 368 Summary
date: 2016-06-29
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组。

**输出**一个List，里面的元素满足条件：任意取两个元素，其中一个可以被另一个整除，且这个List是最大的List

# 我的算法

考虑动态规划算法，若我们已知前n-1个元素的List，求加入第n个元素后的最大List，我们可以遍历前n-1个元素的最大List，找出n能加入，且最大的List即可。

判断n是否能加入List，我们不需要遍历List中的每一个元素，只需要判断是否能被最大元素整除即可。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        if (nums == null || nums.length < 1) {
            return new ArrayList<Integer>();
        }
        
        List<List<Integer>> maxList = new ArrayList<List<Integer>>();
        List<Integer> currentList = null;
        List<Integer> best = null;
        List<Integer> result = new ArrayList<Integer>();
        
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length; i++) {
            currentList = new ArrayList<Integer>();
            best = currentList;
            for (List<Integer> list : maxList) {
                if (((nums[i] % list.get(list.size() - 1)) == 0) && (list.size() > best.size())) {
                    best = list;
                } 
            }
            currentList.addAll(best);
            currentList.add(nums[i]);
            maxList.add(currentList);
            if (currentList.size() > result.size()) {
                result = currentList;
            }
        }
        
        return result;
    }
}
{% endhighlight %}