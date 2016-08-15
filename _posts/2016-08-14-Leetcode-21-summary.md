---
layout: post
title: Leetcode Problem 21 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

# 我的算法

merge 两个有序链表，没什么说的。。。

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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode fakeHead = new ListNode(0);
        ListNode iter = fakeHead;
        
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                iter.next = l1;
                l1 = l1.next;
                iter = iter.next;
            } else {
                iter.next = l2;
                l2 = l2.next;
                iter = iter.next;
            }
        }
        
        iter.next = (l1 == null ? l2 : l1);
        return fakeHead.next;
    }
}
{% endhighlight %}