---
layout: post
title: Leetcode Problem 112 Summary
date: 2016-08-16
categories: blog
tags: [study]

---

# 题目

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,  

![problem112](https://lisencn11.github.io/img/Problem112.png)

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

# 我的算法

明显是树的深度优先遍历问题。

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
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) return false;
        if (root.left == null && root.right == null) {
            if (sum == root.val) return true;
            else return false;
        }
        
        int remain = sum - root.val;
        return hasPathSum(root.left, remain) || hasPathSum(root.right, remain);
    }
}
{% endhighlight %}