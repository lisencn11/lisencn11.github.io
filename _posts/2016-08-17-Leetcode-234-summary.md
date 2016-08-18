---
layout: post
title: Leetcode Problem 234 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Given a singly linked list, determine if it is a palindrome.

Follow up:  
Could you do it in O(n) time and O(1) space?

# 我的算法

对于 O(1) 空间复杂度的要求，我们需要首先找到链表的中点，然后将右半边翻转，然后可以进行对比。

然而我不认为这是个好方法，因为方法中改变了传入的参数是十分不好的行为。

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
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        ListNode fast = head;
        ListNode slow = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode next = slow.next;
        ListNode iter = slow;
        while (next != null) {
            ListNode temp = next.next;
            next.next = iter;
            iter = next;
            next = temp;
        }
        
        ListNode iter1 = head;
        while (iter != slow) {
            if (iter.val != iter1.val) return false;
            iter = iter.next;
            iter1 = iter1.next;
        }
        
        return iter1.val == iter.val;
    }
}
{% endhighlight %}