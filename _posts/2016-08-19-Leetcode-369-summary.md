---
layout: post
title: Leetcode Problem 369 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Given a non-negative number represented as a singly linked list of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

Example:  
Input:  
1->2->3

Output:  
1->2->4

# 我的算法

加一和加法不一样，有一个特殊性质，遇 9 进 1 ，遇其他直接加 1 即可，所以我们可以通过定位最后一个非 9 结点然后将其加 1 ，并将后面的所有元素变为 0 即可。

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
    public ListNode plusOne(ListNode head) {
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode notNine = fakeHead;
        ListNode iter = fakeHead.next;
        
        while (iter != null) {
            notNine = iter.val == 9 ? notNine : iter;
            iter = iter.next;
        }
        
        notNine.val++;
        iter = notNine.next;
        while (iter != null) {
            iter.val = 0;
            iter = iter.next;
        }
        
        return fakeHead.val == 1 ? fakeHead : fakeHead.next;
    }
}
{% endhighlight %}