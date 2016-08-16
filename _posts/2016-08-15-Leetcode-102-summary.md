---
layout: post
title: Leetcode Problem 270 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:  
Given binary tree [3,9,20,null,null,15,7],  
return its level order traversal as:  
[  
  [3],  
  [9,20],  
  [15,7]  
]

# 我的算法

纯考察BFS。

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();
        
        List<List<Integer>> ret = new ArrayList<>();
        Queue<TreeNode> level = new LinkedList<>();
        level.offer(root);
        
        while (!level.isEmpty()) {
            List<Integer> values = new ArrayList<>();
            int len = level.size();
            for (int i = 0; i < len; i++) {
                TreeNode node = level.poll();
                values.add(node.val);
                if (node.left != null) level.offer(node.left);
                if (node.right != null) level.offer(node.right);
            }
            ret.add(values);
        }
        
        return ret;
    }
}
{% endhighlight %}