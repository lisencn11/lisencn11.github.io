---
layout: post
title: Leetcode Problem 160 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

![](https://lisencn11.github.io/img/problem160.png)

begin to intersect at node c1.

Notes:

* If the two linked lists have no intersection at all, return null.
* The linked lists must retain their original structure after the function returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* Your code should preferably run in O(n) time and use only O(1) memory.

# 我的算法

首先第一想法是HashSet做，很简单，但是做不到空间复杂度 O(1) ，考虑如果遍历一遍两个数组得到长度差然后先将长的链表指针移到同一起跑线即可， discuss 中给出了基于这种思路，但是不需要比较长度的算法，一并贴上。

# 代码

{% highlight java %}
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = length(headA);
        int lenB = length(headB);
        
        int diff = 0;
        ListNode iterA = headA;
        ListNode iterB = headB;
        if (lenA > lenB) {
            diff = lenA - lenB;
            for (int i = 0; i < diff; i++) iterA = iterA.next;
        } else {
            diff = lenB - lenA;
            for (int i = 0; i < diff; i++) iterB = iterB.next;
        }
        
        while (iterA != null && iterA != iterB) {
            iterA = iterA.next;
            iterB = iterB.next;
        }
        
        return iterA;
    }
    
    private int length(ListNode head) {
        int len = 0;
        while (head != null) {
            head = head.next;
            len++;
        }
        return len;
    }
}
{% endhighlight %}

# discuss摘录

{% highlight java %}
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    //boundary check
    if(headA == null || headB == null) return null;
    
    ListNode a = headA;
    ListNode b = headB;
    
    //if a & b have different len, then we will stop the loop after second iteration
    while( a != b){
    	//for the end of first iteration, we just reset the pointer to the head of another linkedlist
        a = a == null? headB : a.next;
        b = b == null? headA : b.next;    
    }
    
    return a;
}
{% endhighlight %}