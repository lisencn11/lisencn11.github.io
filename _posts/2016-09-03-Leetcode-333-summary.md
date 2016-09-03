---
layout: post
title: Leetcode Problem 333 Summary
date: 2016-09-03
categories: blog
tags: [study]

---

# 题目

Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

Note:  
A subtree must include all of its descendants.  
Here's an example:

![](https://lisencn11.github.io/img/problem333.png)

The Largest BST Subtree in this case is the highlighted one.   
The return value is the subtree's size, which is 3.

Hint:

1. You can recursively use algorithm similar to 98. Validate Binary Search Tree at each node of the tree, which will result in O(nlogn) time complexity.

Follow up:  
Can you figure out ways to solve it with O(n) time complexity?

# 我的算法

backtrack ，我在这里犯错了，我判断的是根节点和左右子结点的大小，实际应该判断和左子树最大值，右子树最小值的大小。

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
    public int largestBSTSubtree(TreeNode root) {
        return isBST(root).size;
    }
    
    private Result isBST(TreeNode root) {
        if (root == null) return new Result(0, 0, 0, true);
        if (root.left == null && root.right == null) return new Result(1, root.val, root.val, true);
        if (root.left == null || root.right == null) {
            if (root.left == null) {
                Result right = isBST(root.right);
                if (root.val < right.low && right.isBST) {
                    return new Result(right.size + 1, root.val, right.high, true);
                } else {
                    return new Result(right.size, 0, 0, false);
                }
            } else {
                Result left = isBST(root.left);
                if (root.val > left.high && left.isBST) {
                    return new Result(left.size + 1, left.low, root.val, true);
                } else {
                    return new Result(left.size, 0, 0, false);
                }
            }
        }
        
        Result left = isBST(root.left);
        Result right = isBST(root.right);
        if (root.val > left.high && root.val < right.low && left.isBST && right.isBST) {
            return new Result(left.size + right.size + 1, left.low, right.high, true);
        } else {
            return new Result(Math.max(left.size, right.size), 0, 0, false);
        }
    }
    
    class Result {
        int size;
        int low;
        int high;
        boolean isBST;
        public Result(int size, int low, int high, boolean isBST) {
            this.size = size;
            this.low = low;
            this.high = high;
            this.isBST = isBST;
        }
    }
}
{% endhighlight %}