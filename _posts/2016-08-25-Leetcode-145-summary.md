---
layout: post
title: Leetcode Problem 145 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the postorder traversal of its nodes' values.


# 我的算法

iterative 方法，用两个指针 prev 和 node 代表前一个访问结点和当前栈顶结点，然后通过比较两者关系进行判断当前的行为。

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
    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) return new ArrayList<>();
        
        List<Integer> output = new ArrayList<>();
        TreeNode pre = null;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        
        while (!stack.isEmpty()) {
            TreeNode node = stack.peek();
            if (pre == null || pre.left == node || pre.right == node) {
                if (node.left != null) {
                    stack.push(node.left);
                } else if (node.right != null) {
                    stack.push(node.right);
                }
            } else if (node.left == pre) {
                if (node.right != null) {
                    stack.push(node.right);
                }
            } else {
                output.add(node.val);
                stack.pop();
            }
            pre = node;
        }
        
        return output;
    }
}
{% endhighlight %}