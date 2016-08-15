---
layout: post
title: Leetcode Problem 206 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

# 我的算法

最简单明了的算法是 recursion ，因为是一棵 BST 如果 p 和 q 的值都小于当前 root ，问题规约为左子树，都大于规约为右子树，一个大一个小返回 root 即可。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val > p.val && root.val > q.val) {
            return lowestCommonAncestor(root.left, p, q);
        } else if (root.val < p.val && root.val < q.val) {
            return lowestCommonAncestor(root.right, p, q);
        } else {
            return root;
        }
    }
}
{% endhighlight %}