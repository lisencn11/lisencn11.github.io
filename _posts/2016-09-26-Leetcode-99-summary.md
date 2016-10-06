---
layout: post
title: Leetcode Problem 99 Summary
date: 2016-09-26
categories: blog
tags: [study]

---

# 题目

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

Note:  
A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

# 我的算法

中序遍历，查找本应该是单调递增序列中的异常。

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
    private TreeNode first = null;
    private TreeNode second = null;
    private TreeNode pre = new TreeNode(Integer.MIN_VALUE);
    
    public void recoverTree(TreeNode root) {
        traverse(root);
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
    
    private void traverse(TreeNode root) {
        if (root == null) return;
        traverse(root.left);
        if (first == null && pre.val > root.val) {
            first = pre;
        }
        
        if (first != null && pre.val > root.val) {
            second = root;
        }
        
        pre = root;
        traverse(root.right);
    }
}
{% endhighlight %}