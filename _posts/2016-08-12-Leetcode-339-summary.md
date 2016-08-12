---
layout: post
title: Leetcode Problem 339 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example 1:
Given the list [[1,1],2,[1,1]], return 10. (four 1's at depth 2, one 2 at depth 1)

Example 2:
Given the list [1,[4,[6]]], return 27. (one 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4*2 + 6*3 = 27)

# 我的算法

抽象为树的深度优先搜索

# 代码

{% highlight java %}
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        if (nestedList == null || nestedList.size() == 0) return 0;
        
        Stack<List<NestedInteger>> nests = new Stack<>(); 
        Stack<Integer> poss = new Stack<>();
        nests.push(nestedList);
        poss.push(0);
        int sum = 0;

        while (!nests.isEmpty()) {
            List<NestedInteger> currList = nests.pop();
            int currPos = poss.pop();
            while (currPos < currList.size() && currList.get(currPos).isInteger()) {
                sum += currList.get(currPos++).getInteger() * (nests.size() + 1);
            }
            
            if (currPos == currList.size()) {
                continue;
            } else {
                nests.push(currList);
                poss.push(currPos + 1);
                nests.push(currList.get(currPos).getList());
                poss.push(0);
            }
        }
        
        return sum;
    }
}
{% endhighlight %}

# 简化

使用递归更加简单，因为解题时没有抽象成树的搜索，所以没想出来。

{% highlight java %}
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        return helper(nestedList, 1);
    }
    
    private int helper(List<NestedInteger> nestedList, int depth) {
        if (nestedList == null || nestedList.size() == 0) return 0;
        
        int ret = 0;
        for (NestedInteger o : nestedList) {
            ret += o.isInteger() ? o.getInteger() * depth : helper(o.getList(), depth + 1);
        }
        
        return ret;
    }
}
{% endhighlight %}