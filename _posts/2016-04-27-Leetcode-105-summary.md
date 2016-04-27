---
layout: post
title: Leetcode Problem 105 Summary
date: 2016-04-27
categories: blog
tags: [study]

---

# 题目

输入一棵二叉树的先序和中序遍历，构建这棵二叉树。


# 算法

算法与103一样，使用回溯算法，从先序遍历的首个元素构建当前二叉树的根节点，利用中序遍历得到左子树和右子树的范围，然后递归调用自身构建左子树和右子树。其中利用HashMap可以在O(1)的时间复杂度内定位inorder[]中根节点的index。

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
    
    static Map<Integer, Integer> map = new HashMap<Integer, Integer>();
    
    private static TreeNode buildTree(int[] preorder, int ps, int pe, int[] inorder, int is, int ie) {
        if (ps > pe || is > ie) {
            return null;
        }
        
        int rootValue = preorder[ps];
        int index = map.get(new Integer(rootValue));
        TreeNode root = new TreeNode(rootValue);
        
        TreeNode left = buildTree(preorder, ps + 1, ps + index - is, inorder, is, index - 1);
        TreeNode right = buildTree(preorder, ps + index - is + 1, pe, inorder, index + 1, ie);
        
        root.left = left;
        root.right = right;
        return root;
    }
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder == null || preorder.length == 0 || inorder == null || inorder.length != preorder.length) {
            return null;
        }
        
        for (int i = 0; i < inorder.length; i++) {
            map.put(new Integer(inorder[i]), new Integer(i));
        }
        
        TreeNode root = buildTree(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
        return root;
    }
}

```

