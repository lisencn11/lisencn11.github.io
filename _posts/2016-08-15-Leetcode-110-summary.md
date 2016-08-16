---
layout: post
title: Leetcode Problem 110 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

# 我的算法

递归，新的函数 height(TreeNode root) ，如果 root 为根的树是平衡二叉树就返回高度，否则返回 -1

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
    public boolean isBalanced(TreeNode root) {
        return height(root) != -1;
    }
    
    private int height(TreeNode root) {
        if (root == null) return 0;
        
        int left = height(root.left);
        int right = height(root.right);
        if (left == -1 || right == -1) return -1;
        if (Math.abs(left - right) > 1) return -1;
        return Math.max(left, right) + 1;
    }
}
{% endhighlight %}