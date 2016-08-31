---
layout: post
title: Leetcode Problem 117 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

* You may only use constant extra space.

For example,  
Given the following binary tree,

![](https://lisencn11.github.io/img/problem117_1.png)

After calling your function, the tree should look like:

![](https://lisencn11.github.io/img/problem117_2.png)

# 我的算法

同 116 题类似的思路，不同的是我们需要对下一层加入判断是否为 null 的一些操作。

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
        if (root == null) return;
        
        TreeLinkNode parent = root;
        TreeLinkNode pre = null;
        TreeLinkNode childHead = null;
        
        while (parent != null) {
            if (parent.left != null) {
                pre = parent.left;
                childHead = parent.left;
                break;
            }
            if (parent.right != null) {
                pre = parent.right;
                childHead = parent.right;
                break;
            }
            parent = parent.next;
        }
        
        if (parent == null) return;
        if (pre == parent.left && parent.right != null) {
            pre.next = parent.right;
            pre = parent.right;
        }
        parent = parent.next;
        while (parent != null) {
            if (parent.left != null) {
                pre.next = parent.left;
                pre = parent.left;
            }
            if (parent.right != null) {
                pre.next = parent.right;
                pre = parent.right;
            }
            parent = parent.next;
        }
        pre.next = null;
        connect(childHead);
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public class Solution {
    
    //based on level order traversal
    public void connect(TreeLinkNode root) {

        TreeLinkNode head = null; //head of the next level
        TreeLinkNode prev = null; //the leading node on the next level
        TreeLinkNode cur = root;  //current node of current level

        while (cur != null) {
            
            while (cur != null) { //iterate on the current level
                //left child
                if (cur.left != null) {
                    if (prev != null) {
                        prev.next = cur.left;
                    } else {
                        head = cur.left;
                    }
                    prev = cur.left;
                }
                //right child
                if (cur.right != null) {
                    if (prev != null) {
                        prev.next = cur.right;
                    } else {
                        head = cur.right;
                    }
                    prev = cur.right;
                }
                //move to next node
                cur = cur.next;
            }
            
            //move to next level
            cur = head;
            head = null;
            prev = null;
        }
        
    }
}
{% endhighlight %}