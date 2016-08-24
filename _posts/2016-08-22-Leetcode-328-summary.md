---
layout: post
title: Leetcode Problem 328 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

Example:  
Given 1->2->3->4->5->NULL,  
return 1->3->5->2->4->NULL.

Note:  
The relative order inside both the even and odd groups should remain as it was in the input. 
The first node is considered odd, the second node even and so on ...

# 我的算法

简单的链表操作。

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
    public ListNode oddEvenList(ListNode head) {
        if (head == null || head.next == null) return head;
        
        ListNode evenHead = head.next;
        ListNode odd = head;
        ListNode even = head.next;
        while (odd.next != null && odd.next.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        
        odd.next = evenHead;
        return head;
    }
}
{% endhighlight %}