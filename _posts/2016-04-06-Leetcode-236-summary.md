---
layout: post
title: Leetcode Problem 236 Summary
date: 2016-04-06
categories: blog
tags: [study]

---

# 题目描述

输入一棵树，给出根结点和两个结点p和q，需要输出p和q的公共祖先，如果p是q的祖先则直接输出p。

# 思路

recursion方法十分简单，我们只需找到根结点左子树中p和q的公共祖先情况，右子树中p和q的公共祖先情况，然后进行总结即可：

* 当递归到p或q时返回当前结点
* 当左右子树返回都为null时说明左右子树均不含p和q，此时我们返回null
* 当左右子树中一个为null，另一个不为null时说明其中一个至少含有p或q中的一个结点，返回那个不为null的结点
* 当两个子树均不为null时，说明p和q一个在左子树，一个在右子树，则我们需要返回root结点

# Java代码

```javas

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
	public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    	if (root == null || root == p || root == q) {
        	return root;
    	}
    	TreeNode leftNode = lowestCommonAncestor(root.left, p, q);
    	TreeNode rightNode = lowestCommonAncestor(root.right, p, q);
    	if (leftNode == null) {
        	return rightNode;
    	} else {
        	if (rightNode == null) {
            	return leftNode;
        	} else {
            	return root;
        	}
    	}
	}
}

```