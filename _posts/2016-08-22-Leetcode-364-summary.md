---
layout: post
title: Leetcode Problem 364 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the previous question where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

Example 1:  
Given the list [[1,1],2,[1,1]], return 8. (four 1's at depth 1, one 2 at depth 2)

Example 2:  
Given the list [1,[4,[6]]], return 17. (one 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1*3 + 4*2 + 6*1 = 17)

# 我的算法

广度优先搜索，记录每一层的数，然后倒叙加权相加。

# 代码

{% highlight java %}
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class Solution {
    public int depthSumInverse(List<NestedInteger> nestedList) {
        Queue<List<NestedInteger>> queue = new LinkedList<>();
        queue.offer(nestedList);
        List<List<Integer>> list = new ArrayList<>();
        
        while (!queue.isEmpty()) {
            int len = queue.size();
            list.add(new ArrayList<>());
            for (int i = 0; i < len; i++) {
                List<NestedInteger> currList = queue.poll();
                for (int j = 0; j < currList.size(); j++) {
                    if (currList.get(j).isInteger()) {
                        List<Integer> temp = list.get(list.size() - 1);
                        temp.add(currList.get(j).getInteger());
                    } else {
                        queue.offer(currList.get(j).getList());
                    }
                }
            }
        }
        
        int sum = 0;
        for (int i = list.size() - 1, j = 1; i >= 0; i--, j++) {
            List<Integer> currLevel = list.get(i);
            for (Integer num : currLevel) {
                sum += j * num;
            }
        }
        
        return sum;
    }
}
{% endhighlight %}

# discuss

很巧妙的算法，BFS遍历每一层，但是不需要存储遍历过的结点值，每一层加和后，在接下来的每一层都会再加一遍，无需乘法。

{% highlight java %}
public int depthSumInverse(List<NestedInteger> nestedList) {
    int unweighted = 0, weighted = 0;
    while (!nestedList.isEmpty()) {
        List<NestedInteger> nextLevel = new ArrayList<>();
        for (NestedInteger ni : nestedList) {
            if (ni.isInteger())
                unweighted += ni.getInteger();
            else
                nextLevel.addAll(ni.getList());
        }
        weighted += unweighted;
        nestedList = nextLevel;
    }
    return weighted;
}
{% endhighlight %}