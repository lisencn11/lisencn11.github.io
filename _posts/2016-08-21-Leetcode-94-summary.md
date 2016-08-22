---
layout: post
title: Leetcode Problem 94 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the inorder traversal of its nodes' values.

For example:  
Given binary tree [1,null,2,3],

return [1,3,2].

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ret = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode node = root;
        while (!stack.isEmpty() || node != null) {
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            ret.add(node.val);
            node = node.right;
        }
        
        return ret;
    }
}
{% endhighlight %}