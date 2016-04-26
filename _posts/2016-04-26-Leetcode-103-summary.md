---
layout: post
title: Leetcode Problem 103 Summary
date: 2016-04-26
categories: blog
tags: [study]

---

# 题目

输入一棵二叉树，输出二叉树的zigzag遍历


# 算法

使用两个stack，奇数层stack与偶数层stack，以root为第1层的话，奇数层stack对每个节点pop出来后依次将左孩子和右孩子放入偶数层stack中，而偶数层stack则对每个节点一次将右孩子和左孩子放入奇数层stack，直到两个stack均为空。

# 代码

```java

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        if (root == null) {
            return new ArrayList<List<Integer>>();
        }
        
        List<Integer> level = new ArrayList<Integer>();
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        Stack<TreeNode> even = new Stack<TreeNode>();
        Stack<TreeNode> odd = new Stack<TreeNode>();
        TreeNode node = null;
        odd.push(root);
        
        while (!odd.isEmpty() || !even.isEmpty()) {
            while (!odd.isEmpty()) {
                node = odd.pop();
                if (node.left != null) {
                    even.push(node.left);
                }
                if (node.right != null) {
                    even.push(node.right);
                }
                level.add(new Integer(node.val));
            }
            if (!level.isEmpty()) {
                result.add(level);
                level = new ArrayList<Integer>();
            }
            while (!even.isEmpty()) {
                node = even.pop();
                if (node.right != null) {
                    odd.push(node.right);
                }
                if (node.left != null) {
                    odd.push(node.left);
                }
                level.add(node.val);
            }
            if (!level.isEmpty()) {
                result.add(level);
                level = new ArrayList<Integer>();
            }
        }
        
        return result;
    }
}

```

