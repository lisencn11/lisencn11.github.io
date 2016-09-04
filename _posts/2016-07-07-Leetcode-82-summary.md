---
layout: post
title: Leetcode Problem 82 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,  
Given 1->2->3->3->4->4->5, return 1->2->5.  
Given 1->1->1->2->3, return 2->3.

# 我的算法

需要使用三个指针，start指针指向重复元素的第一个元素，end指针指向第一个非重复元素，perStart指针指向start的前一个元素。使用fakeHead以防全部元素均被删除，简化边界条件。len用于记录end和start间长度。

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
        if (head == null || head.next == null) {
            return head;
        }
        
        ListNode fakeHead = new ListNode(0);
        ListNode start;
        ListNode end;
        ListNode preStart;
        int len = 1;
        
        fakeHead.next = head;
        preStart = fakeHead;
        start = preStart.next;
        end = start.next;
        
        while (end != null) {
            if (end.val == start.val) {
                end = end.next;
                len++;
            } else {
                if (len > 1) {
                    preStart.next = end;
                    start = end;
                    end = end.next;
                    len = 1;
                } else {
                    preStart = preStart.next;
                    start = start.next;
                    end = end.next;
                }
            }
        }
        
        if (len > 1) {
            preStart.next = null;
        }
        return fakeHead.next;
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
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return head;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;
        ListNode curr = head;

        while (curr != null) {
            if (curr.next != null && curr.val == curr.next.val) {
                while (curr.next != null && curr.val == curr.next.val) {
                    curr = curr.next;
                }
            } else {
                prev.next = curr;
                prev = curr;
            }
            curr = curr.next;
        }
        prev.next = null;
        return dummy.next;
    }
}
{% endhighlight %}