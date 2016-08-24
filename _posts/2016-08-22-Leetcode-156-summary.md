---
layout: post
title: Leetcode Problem 156 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

For example:
Given a binary tree {1,2,3,4,5},

![](https://lisencn11.github.io/img/problem156_1.png)

return the root of the binary tree [4,5,2,#,#,3,1].

![](https://lisencn11.github.io/img/problem156_2.png)

# 我的算法

感觉 iterative 操作会很麻烦，才用 recursive 解决。

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
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if (root == null) return root;
        TreeNode node = root;
        while (node.left != null) node = node.left;
        flip(root);
        return node;
    }
    
    private TreeNode flip(TreeNode root) {
        if (root == null) return root;
        if (root.left == null && root.right == null) return root;
        
        TreeNode parent = flip(root.left);
        parent.right = root;
        parent.left = root.right;
        root.left = null;
        root.right = null;
        
        return root;
    }
}
{% endhighlight %}

# discuss

discuss 中总是有比我的代码简单整洁的多的代码。。。

{% highlight java %}
public TreeNode upsideDownBinaryTree(TreeNode root) {
  if (root == null || root.left == null && root.right == null)
    return root;

  TreeNode newRoot = upsideDownBinaryTree(root.left);
  
  root.left.left = root.right;
  root.left.right = root;
  
  root.left = null;
  root.right = null;
      
  return newRoot;
}
{% endhighlight %}