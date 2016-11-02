---
layout: post
title: Leetcode Problem 272 Summary
date: 2016-10-30
categories: blog
tags: [study]

---

# 题目

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

Note:

* Given target value is a floating point.
* You may assume k is always valid, that is: k ≤ total nodes.
* You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

Follow up:

Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

Hint:

1. Consider implement these two helper functions:
 1. getPredecessor(N), which returns the next smaller node to N.
 2. getSuccessor(N), which returns the next larger node to N.
2. Try to assume that each node has a parent pointer, it makes the problem much easier.
3. Without parent pointer we just need to keep track of the path from the root to the current node using a stack.
4. You would need two stacks to track the path in finding predecessor and successor node separately.

# 我的算法

以中序遍历BST，先遍历左孩子得到从小到大节点顺序，先遍历右孩子则相反。那么我们使用两个方法分别遍历到target为止，用两个stack存储则得到小于target的节点从近到远，和大于target的节点从近到远，使用merge sort思想即可。

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
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        List<Integer> result = new ArrayList<>();
        Stack<Integer> smallers = new Stack<>();
        Stack<Integer> largers = new Stack<>();
        
        inorder(root, target, smallers, true);
        inorder(root, target, largers, false);
        
        while (k-- > 0) {
            if (smallers.isEmpty()) {
                result.add(largers.pop());
            } else if (largers.isEmpty()) {
                result.add(smallers.pop());
            } else if (Math.abs(smallers.peek() - target) < Math.abs(largers.peek() - target)) {
                result.add(smallers.pop());
            } else {
                result.add(largers.pop());
            }
        }
        
        return result;
    }
    
    private void inorder(TreeNode node, double target, Stack<Integer> stack, boolean smallers) {
        if (node == null) return;
        
        if (smallers) {
            inorder(node.left, target, stack, true);
            if (node.val > target) return;
            stack.push(node.val);
            inorder(node.right, target, stack, true);
        } else {
            inorder(node.right, target, stack, false);
            if (node.val <= target) return;
            stack.push(node.val);
            inorder(node.left, target, stack, false);
        }
    }
}
{% endhighlight %}