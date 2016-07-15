---
layout: post
title: Leetcode Problem 98 Summary
date: 2016-07-15
categories: blog
tags: [study]

---

# 题目

**输入**一棵树的根节点。

**输出**这棵树是否是二叉搜索树。

二叉搜索树左节点小于父节点，右节点大于父节点，且左右子树仍为二叉搜索树。

# 我的算法

我使用的是递归，返回一个数组int[3]分别记录子树当前节点的树是否是二叉搜索树，最小值为多少，最大值为多少。父节点可以根据这些信息进行判断并进一步返回结果直到根节点

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
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        return valid(root)[0] == 0;
    }
    
    private int[] valid(TreeNode root) {
        int[] minMax = new int[3];
        int[] left = null;
        int[] right = null;
        
        if (root.left == null && root.right == null) {
            minMax[0] = 0;
            minMax[1] = root.val;
            minMax[2] = root.val;
        } else if (root.left == null) {
            right = valid(root.right);
            if (right[0] == 0 && root.val < right[1]) {
                minMax[0] = 0;
                minMax[1] = root.val;
                minMax[2] = right[2];
            } else {
                minMax[0] = -1;
            }
        } else if (root.right == null) {
            left = valid(root.left);
            if (left[0] == 0 && root.val > left[2]) {
                minMax[0] = 0;
                minMax[1] = left[1];
                minMax[2] = root.val;
            } else {
                minMax[0] = -1;
            }
        } else {
            left = valid(root.left);
            right = valid(root.right);
            if (left[0] == 0 && right[0] == 0 && root.val > left[2] && root.val < right[1]) {
                minMax[0] = 0;
                minMax[1] = left[1];
                minMax[2] = right[2];
            } else {
                minMax[0] = -1;
            }
        }
        return minMax;
    } 
}
{% endhighlight %}

# 另一种算法

讨论区多用深度优先搜索算法，中序遍历会令我们先左孩子，然后父节点，然后右孩子，只要纪录前一个节点pre，然后比较当前节点cur和pre大小即可，无序之前复杂的分类讨论。

{% highlight java %}
public boolean isValidBST (TreeNode root){
    Stack<TreeNode> stack = new Stack<TreeNode> ();
    TreeNode cur = root ;
    TreeNode pre = null ;		   
    while (!stack.isEmpty() || cur != null) {			   
        if (cur != null) {
            stack.push(cur);
            cur = cur.left ;
        } else {				   
            TreeNode p = stack.pop() ;
            if (pre != null && p.val <= pre.val) {           
                return false ;
            }				   
            pre = p ;					   
            cur = p.right ;
        }
    }
    return true ; 
}
{% endhighlight %}