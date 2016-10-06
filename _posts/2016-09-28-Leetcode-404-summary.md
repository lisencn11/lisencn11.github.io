---
layout: post
title: Leetcode Problem 404 Summary
date: 2016-09-28
categories: blog
tags: [study]

---

# 题目

Find the sum of all left leaves in a given binary tree.

Example:

![](https://lisencn11.github.io/img/problem404.png)

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

# 我的算法

backtrack，得到当前节点为根节点的所有左叶子和。

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
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) return 0;
        else return sumHelper(root.left, root) + sumHelper(root.right, root);
    }
    
    private int sumHelper(TreeNode node, TreeNode parent) {
        if (node == null) return 0;
        if (node.left == null && node.right == null) {
            if (node == parent.left) {
                return node.val;
            } else {
                return 0;
            }
        }
        int left = sumHelper(node.left, node);
        int right = sumHelper(node.right, node);
        return left + right;
    }
}
{% endhighlight %}