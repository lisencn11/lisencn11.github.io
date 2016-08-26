---
layout: post
title: Leetcode Problem 116 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given a binary tree

{% highlight java %}
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
{% endhighlight %}
    
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

* You may only use constant extra space.
* You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,  
Given the following perfect binary tree,

![](https://lisencn11.github.io/img/problem116_1.png)

After calling your function, the tree should look like:

![](https://lisencn11.github.io/img/problem116_2.png)


# 我的算法

backtrack ，尾递归，记录下下一层的首结点，然后利用当前层的 next 的指针，完成下一层的 next 指针，对之前记录的首结点调用相同算法。

# 代码

{% highlight java %}
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null || root.left == null) return;
        
        TreeLinkNode parent = root.next;
        TreeLinkNode pre = root;
        pre.left.next = pre.right;
        TreeLinkNode leftChild = root.left;
        while (parent != null) {
            pre.right.next = parent.left;
            parent.left.next = parent.right;
            pre = parent;
            parent = parent.next;
        }
        connect(leftChild);
    }
}
{% endhighlight %}