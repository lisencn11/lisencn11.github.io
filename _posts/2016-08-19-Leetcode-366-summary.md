---
layout: post
title: Leetcode Problem 266 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

Example:  
Given binary tree 

![problem366](https://lisencn11.github.io/img/problem366.png)

Returns [4, 5, 3], [2], [1].

Explanation:
1. Removing the leaves [4, 5, 3] would result in this tree:

          1
         / 
        2          
2. Now removing the leaf [2] would result in this tree:

          1          
3. Now removing the leaf [1] would result in the empty tree:

          []         
Returns [4, 5, 3], [2], [1].

# 我的算法

用一个HashSet记录已经被删除的结点，对二叉树进行多次深度优先遍历。

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
    public List<List<Integer>> findLeaves(TreeNode root) {
        if (root == null) return new ArrayList<>();
        List<List<Integer>> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        Set<TreeNode> set = new HashSet<>();
        
        while (!set.contains(root)) {
            stack = new Stack<>();
            stack.push(root);
            List<Integer> list = new ArrayList<>();
            while (!stack.isEmpty()) {
                TreeNode node = stack.pop();
                if ((node.left == null || set.contains(node.left)) && (node.right == null || set.contains(node.right))) {
                    list.add(node.val);
                    set.add(node);
                } else {
                    if (node.right != null && !set.contains(node.right)) stack.push(node.right);
                    if (node.left != null && !set.contains(node.left)) stack.push(node.left);
                }
            }
            result.add(list);
        }
        
        return result;
    }
}
{% endhighlight %}

# discuss根据结点高度

我没有抓住这道题的考点，其实这道题在考树的高度这个概念，因为同一高度的结点会放入同一List，discuss中根据这个考点给出了解答。

{% highlight java %}
public class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    
    public List<List<Integer>> findLeaves(TreeNode root) {
        helper(root);
        return result;
    }
    
    private int helper(TreeNode root) {
        if (root == null) return -1;
        int height = 1 + Math.max(helper(root.left), helper(root.right));
        if (result.size() <= height) result.add(new ArrayList<>());
        result.get(height).add(root.val);
        return height;
    }
}
{% endhighlight %}