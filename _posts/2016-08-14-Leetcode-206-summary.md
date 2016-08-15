---
layout: post
title: Leetcode Problem 206 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Reverse a singly linked list

# 我的算法

最简单的就是重新建一个链表逆序。

原地逆序的方法图解：

1->2->3->4->null 变成

1<-2 1->3->4->null 变成

1<-2<-3 1->4->null

重复这个过程直到

1<-2<-3<-4 1->null

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
    public ListNode reverseList(ListNode head) {
        if (head == null) return head;
        
        ListNode next  = head.next;
        ListNode iter = head;
        ListNode pre = null;
        while (next != null) {
            pre = iter;
            iter = next;
            next = next.next;
            head.next = next;
            iter.next = pre;
        }
        
        return iter;
    }
}
{% endhighlight %}