---
layout: post
title: Leetcode Problem 114 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a binary tree, flatten it to a linked list in-place.

For example,

Given

![](https://lisencn11.github.io/img/problem114_1.png)

The flattened tree should look like:

![](https://lisencn11.github.io/img/problem114_2.png)

# 我的算法

backtrack

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
    public void flatten(TreeNode root) {
        helper(root);
    }
    
    private TreeNode helper(TreeNode root) {
        if (root == null) return null;
        if (root.left == null && root.right == null) return root;
        
        TreeNode left = helper(root.left);
        TreeNode right = helper(root.right);
        
        if (left == null) return root;
        root.right = left;
        root.left = null;
        while (left.right != null) left = left.right;
        left.right = right;
        return root;
    }
}
{% endhighlight %}