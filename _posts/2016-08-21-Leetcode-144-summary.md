---
layout: post
title: Leetcode Problem 144 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the preorder traversal of its nodes' values.

For example:  
Given binary tree {1,#,2,3},

return [1,2,3].

Note: Recursive solution is trivial, could you do it iteratively?

# 我的算法

给出 recursive 和 iterative 解法。

# recursive

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
    List<Integer> ret = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) return ret;
        
        ret.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        
        return ret;
    }
}
{% endhighlight %}

# iterative

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
    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) return new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> ret = new ArrayList<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            ret.add(node.val);
            if (node.right != null) stack.push(node.right);
            if (node.left != null) stack.push(node.left);
        }
        
        return ret;
    }
}
{% endhighlight %}