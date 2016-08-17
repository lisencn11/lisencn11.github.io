---
layout: post
title: Leetcode Problem 19 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
   
Note:

Given n will always be valid.  
Try to do this in one pass.

# 我的算法

two pointers，距离为 n + 1 ，直到前一个为 null 的时候，后一个 pointer 将其之后的结点删除即可。

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode fastNode = fakeHead;
        ListNode slowNode = fakeHead;
        
        for (int i = 0; i <= n; i++) fastNode = fastNode.next;
        
        while (fastNode != null) {
            fastNode = fastNode.next;
            slowNode = slowNode.next;
        }
        
        slowNode.next = slowNode.next.next;
        
        return fakeHead.next;
    }
}
{% endhighlight %}