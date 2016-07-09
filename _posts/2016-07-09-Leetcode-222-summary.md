---
layout: post
title: Leetcode Problem 222 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

**输入**一个完全二叉树（除最后一层外每层都满，最后一层从左到右依次填充）的根节点。

**输出**这棵二叉树的总节点数。

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