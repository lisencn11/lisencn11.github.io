---
layout: post
title: Leetcode Problem 298 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,

![](https://lisencn11.github.io/img/problem298_1.png)

Longest consecutive sequence path is 3-4-5, so return 3.

![](https://lisencn11.github.io/img/problem298_2.png)

Longest consecutive sequence path is 2-3,not3-2-1, so return 2.

# 我的算法

递归解决二叉树。helper() 会返回当前节点为起始的连续序列长度，若此长度大于最大长度，就修改一个全局变量 max。

# 代码

{% highlight java %}
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    int max = 0;
    public int longestConsecutive(TreeNode root) {
        helper(root);
        return max;
    }
    
    private int helper(TreeNode root) {
        if (root == null) return 0;

        int left = helper(root.left);
        int right = helper(root.right);
        int len = 1;
        if (root.left != null && root.left.val - 1 == root.val) len = left + 1;
        if (root.right != null && root.right.val - 1 == root.val) len = right + 1 > len ? right + 1 : len;
        max = len > max ? len : max;
        return len;
    }
}
{% endhighlight %}