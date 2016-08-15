---
layout: post
title: Leetcode Problem 21 Summary
date: 2016-08-14
categories: blog
tags: [study]

---

# 题目

Given a linked list, determine if it has a cycle in it.

Follow up:  
Can you solve it without using extra space?

# 我的算法

非常普通的一道题，刚开始打算用 HashSet 解决，看到 Follow up 是不用额外空间，想到可以使用两个 pointer 的方法，一个快一个慢，如果存在 cycle 的话，快的那个会在某个时刻追上慢的，不存在的话快的会首先到达 null。

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
    public boolean hasCycle(ListNode head) {
        ListNode p1 = head;
        ListNode p2 = head;
        
        while (p2 != null && p2.next != null) {
            p1 = p1.next;
            p2 = p2.next.next;
            if (p1 == p2) return true;
        }
        return false;
    }
}
{% endhighlight %}