---
layout: post
title: Leetcode Problem 148 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

**输入**一个整型链表。

**输出**排序后的整型链表。

要求排序的时间复杂度为O(nlogn)，空间复杂度为O(1)。

# 我的算法

归并排序应用于链表上。这里给出自顶向下的归并排序，严格说的话我们这里仍然用了O(logn)的空间复杂度在栈空间上，可以通过改写为自底向上的归并排序。

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
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;
        
        ListNode pre = null;
        ListNode p = head;
        ListNode f = head;
        while (p != null && p.next != null) {
            pre = f;
            f = f.next;
            p = p.next.next;
        }
        
        pre.next = null;
        ListNode h2 = sortList(f);
        ListNode h1 = sortList(head);
        return merge(h1, h2);
    }
    
    private ListNode merge(ListNode h1, ListNode h2) {
        ListNode fakeHead = new ListNode(0);
        ListNode iter = fakeHead;
        while (h1 != null && h2 != null) {
            if (h1.val < h2.val) {
                iter.next = h1;
                h1 = h1.next;
            } else {
                iter.next = h2;
                h2 = h2.next;
            }
            iter = iter.next;
        }
        if (h1 != null) iter.next = h1;
        if (h2 != null) iter.next = h2;
        return fakeHead.next;
    }
}
{% endhighlight %}