---
layout: post
title: Leetcode Problem 107 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:  
Given binary tree [3,9,20,null,null,15,7],

return its bottom-up level order traversal as:  
[  
  [15,7],  
  [9,20],  
  [3]  
]

# 我的算法

广度优先遍历配合Stack可以实现

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if (root == null) return new ArrayList<>();
        List<List<Integer>> ret = new ArrayList<>();
        Stack<List<Integer>> stack = new Stack<>();
        Queue<TreeNode> queue = new LinkedList<>();
        
        queue.offer(root);
        while (!queue.isEmpty()) {
            int len = queue.size();
            List<Integer> curr = new ArrayList<>();
            for (int i = 0; i < len; i++) {
                TreeNode iter = queue.poll();
                curr.add(iter.val);
                if (iter.left != null) queue.offer(iter.left);
                if (iter.right != null) queue.offer(iter.right);
            }
            stack.push(curr);
        }
        
        while (!stack.isEmpty())
            ret.add(stack.pop());
            
        return ret;
    }
}
{% endhighlight %}