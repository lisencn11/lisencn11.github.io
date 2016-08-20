---
layout: post
title: Leetcode Problem 382 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

Follow up:  
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

Example:

// Init a singly linked list [1,2,3].  
ListNode head = new ListNode(1);  
head.next = new ListNode(2);  
head.next.next = new ListNode(3);  
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.  
solution.getRandom();

# 我的算法

最简单的算法是计算长度后得到随机 index，但并不是这道题的考点。这道题主要考察 [reservoir sampling](http://www.cnblogs.com/HappyAngel/archive/2011/02/07/1949762.html)。

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
    ListNode head;
    Random randomGenerator;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        this.head = head;
        randomGenerator = new Random();
    }
    
    /** Returns a random node's value. */
    public int getRandom() {
        if (head == null) return 0;
        int random = head.val;
        ListNode node = head.next;
        int count = 1;
        while (node != null) {
            count++;
            double bound = 1.0 / (double)count;
            if (randomGenerator.nextDouble() < bound) random = node.val;
            node = node.next;
        }
        return random;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
{% endhighlight %}