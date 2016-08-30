---
layout: post
title: Leetcode Problem 86 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,  
Given 1->4->3->2->5->2 and x = 3,  
return 1->2->2->4->3->5.

# 我的算法

two pointer，一个小于元素的开头，一个大于等于元素的开头。

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
    public ListNode partition(ListNode head, int x) {
        ListNode less = new ListNode(0);
        ListNode greater = new ListNode(0);
        ListNode iter = head;
        ListNode lessIter = less;
        ListNode greaterIter = greater;
        
        while (iter != null) {
            if (iter.val < x) {
                lessIter.next = iter;
                lessIter = lessIter.next;
            } else {
                greaterIter.next = iter;
                greaterIter = greaterIter.next;
            }
            
            iter = iter.next;
        }
        
        lessIter.next = greater.next;
        greaterIter.next = null;
        return less.next;
    }
}
{% endhighlight %}