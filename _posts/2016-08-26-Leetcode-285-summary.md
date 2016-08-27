---
layout: post
title: Leetcode Problem 285 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return null.

# 我的算法

一开始的想法是做中序遍历，但是发现这样的话 BST 这个条件没有用上。

考虑如果是 BST 的话， p 的中序遍历的后继节点其实就是按大小排序后 p 的下一个结点，可能是 p 的右子树的最小值，也可能是 p 的某个祖先节点，p 需要在这个祖先节点的左子树。

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
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (p.right != null) {
            TreeNode iter = p.right;
            while (iter.left != null) iter = iter.left;
            return iter;
        }
        
        TreeNode iter = root;
        Stack<TreeNode> stack = new Stack<>();
        while (iter != p) {
            if (iter.val < p.val) {
                stack.push(iter);
                iter = iter.right;
            } else {
                stack.push(iter);
                iter = iter.left;
            }
        }
        
        while (!stack.isEmpty()) {
            TreeNode parent = stack.pop();
            if (iter == parent.right) iter = parent;
            else {
                return parent;
            }
        }
        
        return null;
    }
}
{% endhighlight %}

# discuss

recursion 解法

{% highlight java %}
public TreeNode successor(TreeNode root, TreeNode p) {
  if (root == null)
    return null;

  if (root.val <= p.val) {
    return successor(root.right, p);
  } else {
    TreeNode left = successor(root.left, p);
    return (left != null) ? left : root;
  }
}
{% endhighlight %}