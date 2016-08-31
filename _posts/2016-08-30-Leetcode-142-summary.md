---
layout: post
title: Leetcode Problem 142 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:  
Can you solve it without using extra space?

# 我的算法

two pointer 一个 fast 一个 slow，碰到一起说明有 loop，然后fast 从 head 开始，slow 从下一个开始相同速度知道碰面即为起点。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) return head;
        ListNode fast = head.next;
        ListNode slow = head;
        
        while (slow != fast && fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        if (slow != fast) return null;
        fast = head;
        slow = slow.next;
        while (slow != fast) {
            fast = fast.next;
            slow = slow.next;
        }
        return fast;
    }
}
{% endhighlight %}