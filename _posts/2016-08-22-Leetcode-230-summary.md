---
layout: post
title: Leetcode Problem 230 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note:   
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

Follow up:  
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

Hint:

Try to utilize the property of a BST.  
What if you could modify the BST node's structure?
The optimal runtime complexity is O(height of BST).

# 我的算法

如果是反复查找的话，我会在 TreeNode 类中加入 rank 域表示其左边有多少结点，或者记录其是第 kth 小的结点，但是这题是一次性的，也就是说没必要去建立新的数据结构，直接中序 DFS 遍历就好。

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
    int i;
    public int kthSmallest(TreeNode root, int k) {
        i = 0;
        int value = find(root, k);
        return value;
    }
    
    private int find(TreeNode root, int k) {
        if (root == null) return -1;
        
        int value = find(root.left, k);
        if (value != -1) return value;
        i++;
        if (i == k) return root.val;
        value = find(root.right, k);
        return value;
    }
}
{% endhighlight %}