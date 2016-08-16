---
layout: post
title: Leetcode Problem 270 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:  
Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.

# 我的算法

简单考察BST，track 和 target 的最小距离以及对应的结点值，根据 target 和当前 root 的值的大小关系确定下一个结点是左子树还是右子树。

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
    public int closestValue(TreeNode root, double target) {
        TreeNode iter = root;
        int ret = Integer.MAX_VALUE;
        double diff = Double.MAX_VALUE;
        
        while (iter != null) {
            double newDiff = Math.abs(iter.val - target);
            if (newDiff < diff) {
                ret = iter.val;
                diff = newDiff;
            }
            if (iter.val < target) iter = iter.right;
            else if (iter.val > target) iter = iter.left;
            else break;
        }
        
        return ret;
    }
}
{% endhighlight %}