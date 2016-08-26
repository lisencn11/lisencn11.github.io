---
layout: post
title: Leetcode Problem 173 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

# 我的算法

hasNext() 为 O(h) next() 为 O(1) 说明每次 hasNext() 不仅需要判断是否有 next 还需要定位到next。结合 BST 的性质，做二叉树的中序遍历即可。

# 代码

{% highlight java %}
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    int next;
    TreeNode root;
    Stack<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        this.root = root;
        stack = new Stack<>();
        if (root == null) return;
        stack.push(root);
        TreeNode node = root.left;
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        if (stack.isEmpty()) return false;
        
        TreeNode small = stack.pop();
        next = small.val;
        if (small.right != null) {
            TreeNode node = small.right;
            while (node != null) {
                stack.push(node);
                node = node.left;
            }
        }
        return true;
    }

    /** @return the next smallest number */
    public int next() {
        return next;
    }
}

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
{% endhighlight %}