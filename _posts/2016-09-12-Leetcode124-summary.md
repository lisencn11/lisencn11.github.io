---
layout: post
title: Leetcode Problem 124 Summary
date: 2016-09-12
categories: blog
tags: [study]

---

# 题目

Given a binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path does not need to go through the root.

For example:  
Given the below binary tree,

       1
      / \
     2   3
     
Return 6.

# 我的算法

backtrack 返回一个 Result 对象，里面包含当前节点内的最大路径和，以及以当前节点为起点的最大路径。注意最大 path 未必过 root，但至少要有一个节点。

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
    public int maxPathSum(TreeNode root) {
        Result result = getResult(root);
        return result.result;
    }
    
    private Result getResult(TreeNode root) {
        if (root == null) return new Result(0, 0);
        if (root.left == null && root.right == null) return new Result(root.val, root.val);
        if (root.left == null) {
            Result right = getResult(root.right);
            int path = Math.max(root.val + right.path, root.val);
            int max = Math.max(path, right.result);
            return new Result(path, max);
        }
        if (root.right == null) {
            Result left = getResult(root.left);
            int path = Math.max(root.val + left.path, root.val);
            int max = Math.max(path, left.result);
            return new Result(path, max);
        }
        Result left = getResult(root.left);
        Result right = getResult(root.right);
        int path = Math.max(Math.max(left.path, right.path) + root.val, root.val);
        int max = Math.max(left.result, right.result);
        if (left.path < 0 && right.path < 0) {
            max = Math.max(max, root.val);
        } else if (left.path < 0) {
            max = Math.max(max, root.val + right.path);
        } else if (right.path < 0) {
            max = Math.max(max, root.val + left.path);
        } else {
            max = Math.max(max, root.val + left.path + right.path);
        }
        return new Result(path, max);
    }
    
    class Result {
        int path;
        int result;
        public Result(int path, int result) {
            this.path = path;
            this.result = result;
        }
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    int maxValue;
    
    public int maxPathSum(TreeNode root) {
        maxValue = Integer.MIN_VALUE;
        maxPathDown(root);
        return maxValue;
    }
    
    private int maxPathDown(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, maxPathDown(node.left));
        int right = Math.max(0, maxPathDown(node.right));
        maxValue = Math.max(maxValue, left + right + node.val);
        return Math.max(left, right) + node.val;
    }
}
{% endhighlight %}