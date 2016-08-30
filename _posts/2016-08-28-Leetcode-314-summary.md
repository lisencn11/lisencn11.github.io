---
layout: post
title: Leetcode Problem 314 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

Examples:

Given binary tree [3,9,20,null,null,15,7],

![](https://lisencn11.github.io/img/problem314_1.png)

return its vertical order traversal as:
[
  [9],
  [3,15],
  [20],
  [7]
]
Given binary tree [3,9,8,4,0,1,7],

![](https://lisencn11.github.io/img/problem314_2.png)

return its vertical order traversal as:
[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]
Given binary tree [3,9,8,4,0,1,7,null,null,null,2,5] (0's right child is 2 and 1's left child is 5),

![](https://lisencn11.github.io/img/problem314_3.png)

return its vertical order traversal as:
[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]

# 我的算法

BFS 加 HashMap

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
    public List<List<Integer>> verticalOrder(TreeNode root) {
        if (root == null) return new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        Queue<Integer> cols = new LinkedList<>();
        Map<Integer, List<Integer>> map = new HashMap<>();
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        
        queue.offer(root);
        cols.offer(0);
        
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            int col = cols.poll();
            min = col < min ? col : min;
            max = col > max ? col : max;
            if (!map.containsKey(col)) map.put(col, new ArrayList<>());
            map.get(col).add(node.val);
            if (node.left != null) {
                queue.offer(node.left);
                cols.offer(col - 1);
            }
            if (node.right != null) {
                queue.offer(node.right);
                cols.offer(col + 1);
            }
        }
        
        int len = max - min + 1;
        List<List<Integer>> ret = new ArrayList<>();
        for (int i = 0; i < len; i++) ret.add(new ArrayList<>());
        
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            int index = entry.getKey() - min;
            ret.set(index, entry.getValue());
        }
        
        return ret;
    }
}
{% endhighlight %}