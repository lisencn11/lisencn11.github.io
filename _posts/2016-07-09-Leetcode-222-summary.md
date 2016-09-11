---
layout: post
title: Leetcode Problem 222 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:

In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

# 我的算法

分别计算当前根节点高度和右子树高度：如果相差为1，说明左子树是完全二叉树，整个二叉树的高度等于左子树的高度加上递归调用函数计算的右子树的高度；如果相差不为1，说明右子树是完全二叉树，整个二叉树的高度等于右子树的高度加上递归调用函数计算的左子树的高度。

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
    private int height(TreeNode root) {
        TreeNode iter = root;
        int h = 0;
        while (iter != null) {
            iter = iter.left;
            h++;
        }
        return h;
    }
    
    public int countNodes(TreeNode root) {
        int h = height(root);
        if (h < 2) {
            return h;
        }
        if (height(root.right) == (h - 1)) {
            return (1 << (h - 1)) + countNodes(root.right);
        } else {
            return (1 << (h - 2)) + countNodes(root.left);
        }
    }
}
{% endhighlight %}

# 二刷

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
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        if (root.left == null && root.right == null) return 1;
        int leftLeft = getLeftDepth(root.left);
        int leftRight = getRightDepth(root.left);
        if (leftRight == leftLeft) {
            return (1 << leftLeft) + countNodes(root.right);
        } else {
            return (1 << (leftLeft - 1)) + countNodes(root.left);
        }
    }
    
    private int getLeftDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            depth++;
            root = root.left;
        }
        return depth;
    }
    
    private int getRightDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            depth++;
            root = root.right;
        }
        return depth;
    }
}
{% endhighlight %}