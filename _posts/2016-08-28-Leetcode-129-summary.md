---
layout: post
title: Leetcode Problem 129 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

1  
/ \  
2 3

The root-to-leaf path 1->2 represents the number 12.  
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.

# 我的算法

recursion

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
    int sum = 0;
    public int sumNumbers(TreeNode root) {
        if (root == null) return 0;
        helper(root, 0);
        return sum;
    }
    
    private void helper(TreeNode root, int pre) {
        if (root.left == null && root.right == null) {
            sum += pre * 10 + root.val;
            return;
        }
        
        int val = pre * 10 + root.val;
        if (root.left != null) helper(root.left, val);
        if (root.right != null) helper(root.right, val);
    }
}
{% endhighlight %}