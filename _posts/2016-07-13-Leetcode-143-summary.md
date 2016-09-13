---
layout: post
title: Leetcode Problem 143 Summary
date: 2016-07-13
categories: blog
tags: [study]

---

# 题目

Given a singly linked list L: L0→L1→…→Ln-1→Ln,  
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes' values.

For example,  
Given {1,2,3,4}, reorder it to {1,4,2,3}.

# 我的算法

算法分为三步：

1. 找到单链表中点 1→2→3→4→5→6
2. 逆转单链表后半段 1→2→3→6→5→4
3. 完成转换 1→6→2→5→3→4

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
    public void reorderList(ListNode head) {
        if (head == null || head.next == null || head.next.next == null) return;
        
        // 找中点
        ListNode fast = head;
        ListNode slow = head;
        ListNode preSlow = slow;
        while (fast != null && fast.next != null) {
            preSlow = slow;
            fast = fast.next.next;
            slow = slow.next;
        }
        
        // 逆转后半段
        while (slow.next != null) {
            ListNode current = slow.next;
            slow.next = current.next;
            current.next = preSlow.next;
            preSlow.next = current;
        }
        
        // 完成转换
        ListNode iter1 = head;
        ListNode iter2 = preSlow.next;
        while (iter1 != preSlow) {
            preSlow.next = iter2.next;
            iter2.next = iter1.next;
            iter1.next = iter2;
            iter1 = iter2.next;
            iter2 = preSlow.next;
        }
    }
}
{% endhighlight %}

# 二刷

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
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;
        ListNode dummy = new ListNode(0);
        ListNode fast = dummy;
        ListNode slow = dummy;
        dummy.next = head;
        while (fast!= null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode secondHead = slow.next;
        slow.next = null;
        ListNode iter = secondHead;
        ListNode next = iter.next;
        while (secondHead.next != null) {
            secondHead.next = next.next;
            next.next = iter;
            iter = next;
            next = secondHead.next;
        }
        secondHead = iter;
        
        ListNode iter1 = head;
        ListNode iter2 = secondHead;
        iter = dummy;
        while (iter1 != null || iter2 != null) {
            if (iter1 != null) {
                iter.next = iter1;
                iter1 = iter1.next;
                iter = iter.next;
            }
            if (iter2 != null) {
                iter.next = iter2;
                iter2 = iter2.next;
                iter = iter.next;
            }
        }
        iter.next = null;
    }
}
{% endhighlight %}