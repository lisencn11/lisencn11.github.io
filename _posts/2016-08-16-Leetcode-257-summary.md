---
layout: post
title: Leetcode Problem 257 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

![](https://lisencn11.github.io/img/problem257.png)

All root-to-leaf paths are:

["1->2->5", "1->3"]

# 我的算法

树的深度优先遍历算法应用。

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
    public List<String> binaryTreePaths(TreeNode root) {
        if (root == null) return new ArrayList<>();
        
        List<String> ret = null;
        if (root.left == null && root.right == null) {
            ret = new ArrayList<>();
            String s = Integer.toString(root.val);
            ret.add(s);
            return ret;
        }
        
        List<String> left = binaryTreePaths(root.left);
        List<String> right = binaryTreePaths(root.right);
        left.addAll(right);
        ret = new ArrayList<>();
        for (String s : left) {
            ret.add(Integer.toString(root.val) + "->" + s);
        }
        
        return ret;
    }
}
{% endhighlight %}