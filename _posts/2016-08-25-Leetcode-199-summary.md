---
layout: post
title: Leetcode Problem 199 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:  
Given the following binary tree,

![](https://lisencn11.github.io/img/problem199.png)

You should return [1, 3, 4].

# 我的算法

bfs 取最右元素，或者 dfs 判断是否是当前深度的首个元素，dfs 用递归比较好追踪深度，如果用 iterative，需要多一个 Stack 用来追踪深度。

# 代码

BFS

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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ret = new ArrayList<>();
        if (root == null) return ret;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int len = queue.size();
            ret.add(queue.peek().val);
            for (int i = 0; i < len; i++) {
                TreeNode node = queue.poll();
                if (node.right != null) queue.offer(node.right);
                if (node.left != null) queue.offer(node.left);
            }
        }
        
        return ret;
    }
}
{% endhighlight %}

# discuss

DFS

{% highlight java %}
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        rightView(root, result, 0);
        return result;
    }
    
    public void rightView(TreeNode curr, List<Integer> result, int currDepth){
        if(curr == null){
            return;
        }
        if(currDepth == result.size()){
            result.add(curr.val);
        }
        
        rightView(curr.right, result, currDepth + 1);
        rightView(curr.left, result, currDepth + 1);
        
    }
}
{% endhighlight %}