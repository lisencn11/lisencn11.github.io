---
layout: post
title: Leetcode Problem 226 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Invert a binary tree.

![from](https://raw.githubusercontent.com/lisencn11/lisencn11.github.io/master/img/Screen%20Shot%202016-08-12%20at%205.09.18%20PM.png)

to

![to](https://raw.githubusercontent.com/lisencn11/lisencn11.github.io/master/img/Screen%20Shot%202016-08-12%20at%205.09.22%20PM.png)

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