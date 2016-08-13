---
layout: post
title: Leetcode Problem 237 Summary
date: 2016-08-12
categories: blog
tags: [study]

---

# 题目

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

# 我的算法

删除当前结点，需要把下一个节点的值复制到当前节点，然后删除下一个节点。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
{% endhighlight %}