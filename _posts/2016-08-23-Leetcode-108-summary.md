---
layout: post
title: Leetcode Problem 108 Summary
date: 2016-08-23
categories: blog
tags: [study]

---

# 题目

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

# 我的算法

recursive 最好做，把左子树和右子树转化好后连接就可以了，利用平衡二叉查找树的性质，数组中间的元素为根即可。

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
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums == null || nums.length == 0) return null;
        TreeNode root = transform(nums, 0, nums.length - 1);
        return root;
    }
    
    private TreeNode transform(int[] nums, int start, int end) {
        if (start > end) return null;
        if (start == end) {
            TreeNode root = new TreeNode(nums[start]);
            return root;
        }
        int mid = start + (end - start) / 2;
        TreeNode left = transform(nums, start, mid - 1);
        TreeNode right = transform(nums, mid + 1, end);
        TreeNode root = new TreeNode(nums[mid]);
        root.left = left;
        root.right = right;
        return root;
    }
}
{% endhighlight %}