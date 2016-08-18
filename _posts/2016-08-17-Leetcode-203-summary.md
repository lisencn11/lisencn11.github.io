---
layout: post
title: Leetcode Problem 203 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Remove all elements from a linked list of integers that have value val.

Example  
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6  
Return: 1 --> 2 --> 3 --> 4 --> 5

# 我的算法

简单链表操作，注意 new 一个 fake head 避免边界条件就好。

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode fakeNode = new ListNode(0);
        fakeNode.next = head;
        
        ListNode iter = fakeNode;
        while (iter.next != null) {
            if (iter.next.val == val) {
                iter.next = iter.next.next;
            } else {
                iter = iter.next;
            }
        }
        
        return fakeNode.next;
    }
}
{% endhighlight %}