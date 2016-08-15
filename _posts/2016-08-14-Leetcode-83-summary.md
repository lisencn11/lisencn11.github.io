---
layout: post
title: Leetcode Problem 83 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,  
Given 1->1->2, return 1->2.  
Given 1->1->2->3->3, return 1->2->3.

# 我的算法

使用两个 pointer ，一个 pre 指向 duplicates 的第一个元素，另一个 iter 指向 duplicates 后的下一个不同元素，然后 pre -> iter 即可。

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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return head;
        
        ListNode pre = head;
        ListNode iter = head.next;
        
        while (iter != null) {
            if (pre.val == iter.val) {
                iter = iter.next;
                continue;
            }
            pre.next = iter;
            pre = iter;
            iter = iter.next;
        }
        pre.next = null;
        
        return head;
    }
}
{% endhighlight %}