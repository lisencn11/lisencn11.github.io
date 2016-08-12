---
layout: post
title: Leetcode Problem 226 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Invert a binary tree.



# 我的算法

本质是考察树的遍历，对每一个节点都交换左右孩子即可。

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
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
{% endhighlight %}