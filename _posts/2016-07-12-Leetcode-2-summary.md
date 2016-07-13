---
layout: post
title: Leetcode Problem 2 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

**输入**两个ListNode，代表两个整型的各位，倒序存储，即第一个元素是整型的最后一位。

**输出**两个整型的加和，仍然是ListNode表示。

如：  
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)  
Output: 7 -> 0 -> 8
# 我的算法

从左到右对位相加，同时处理进位即可。最后处理特例，即一条链长，长的那条一直进位知道产生一个新位。

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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        }
        
        ListNode iter1 = l1, iter2 = l2;
        ListNode fakeHead = new ListNode(0);
        ListNode pre = fakeHead;
        int carry = 0, sum = 0;
        while (iter1 != null && iter2 != null) {
            sum = iter1.val + iter2.val + carry;
            carry = sum / 10;
            ListNode newNode = new ListNode(sum % 10);
            pre.next = newNode;
            pre = newNode;
            iter1 = iter1.next;
            iter2 = iter2.next;
        }
        
        ListNode iter = (iter1 == null) ? iter2 : iter1;
        pre.next = iter;
        while (iter != null && carry != 0) {
            sum = iter.val + carry;
            carry = sum / 10;
            iter.val = sum % 10;
            pre = iter;
            iter = iter.next;
        }
        
        if (carry != 0 && iter == null) {
            ListNode newNode = new ListNode(carry);
            pre.next = newNode;
            newNode.next = null;
        }
        return fakeHead.next;
    }
}
{% endhighlight %}