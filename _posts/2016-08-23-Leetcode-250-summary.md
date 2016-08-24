---
layout: post
title: Leetcode Problem 250 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

For example:  
Given binary tree,

![](https://lisencn11.github.io/img/problem250.png)

return 4.

# 我的算法

递归算法，返回值为一个数组 int[] ret，ret[0] 记录子树中的 unival 数量， ret[1] 记录子树是否是 unival ，0 表示不是，1 表示是。

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
    public int countUnivalSubtrees(TreeNode root) {
        if (root == null) return 0;
        int[] result = helper(root);
        return result[0];
    }
    
    private int[] helper(TreeNode root) {
        if (root == null) return null;
        
        int[] left = helper(root.left);
        int[] right = helper(root.right);
        int count = 0;
        boolean unival = true;
        int[] ret = new int[2];
        if (left != null) {
            count += left[0];
            if (unival && (left[1] == 0 || root.val != root.left.val)) unival = false;
        }
        if (right != null) {
            count += right[0];
            if (unival && (right[1] == 0 || root.val != root.right.val)) unival = false;
        }
        ret[0] = unival ? count + 1 : count;
        ret[1] = unival ? 1 : 0;

        return ret;
    }
}
{% endhighlight %}