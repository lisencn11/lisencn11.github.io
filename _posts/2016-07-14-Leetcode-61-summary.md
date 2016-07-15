---
layout: post
title: Leetcode Problem 61 Summary
date: 2016-07-14
categories: blog
tags: [study]

---

# 题目

**输入**一个链表。

**输出**向右旋转链表k位。

如：  
>Given 1->2->3->4->5->NULL and k = 2,  
return 4->5->1->2->3->NULL.

# 我的算法

1. 从头到尾遍历一边计算节点数
2. 使用节点数和k计算旋转几位（因为k可能大于链表长度）
3. 遍历到新头和新尾，对链表进行重链接

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) return head;
        
        int counter = 1;
        ListNode iter = head;
        while (iter.next != null) {
            iter = iter.next;
            counter++;
        }
        
        ListNode tail = iter;
        int shift = k % counter, start = counter - shift;
        if (shift == 0) return head;
        
        iter = head;
        for (int i = 1; i < start; i++) 
            iter = iter.next;
            
        ListNode endNode = iter;
        ListNode startNode = iter.next;
        endNode.next = tail.next;
        tail.next = head;
        return startNode;
    }
}
{% endhighlight %}