---
layout: post
title: Leetcode Problem 92 Summary
date: 2016-06-13
categories: blog
tags: [study]

---

# 题目

**输入**一个整型链表，起始位置m，终止位置n。

**输出**将原链表从起始位置到终止位置逆转。

输入：1->2->3->4->5->null, m = 2, n = 4;

输出：1->4->3->2->5->null

要求**in-place & in one-pass**

# 我的算法

需要多个引用，node引用遍历链表；next引用用于记录下一个引用，防止逆转后丢失引用，pre引用用于记录逆转部分之前的引用。此题要注意一些特例，比如逆转部分包含头，所以我们需要一个fake head；又比如逆转部分包含尾，所以我们最后要判断node是否为空。

# 代码

```java

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (m == n || head == null) {
            return head;
        }
        ListNode fakeHead = new ListNode(0);
        ListNode pre = null, next = null, node = null, reverseHead = null, reverseTail = null;
        
        fakeHead.next = head;
        node = fakeHead;
        for (int i = 0; i <= n; i++) {
            if (i < m) {
                pre = node;
                node = node.next;
                next = node.next;
            } else if (i == m) {
                reverseHead = pre;
                pre = node;
                node = node.next;
                next = node.next;
            } else if (i > m && i <= n) {
                node.next = pre;
                pre = node;
                node = next;
                if (node != null) {
                    next = node.next;
                } else {
                    next = null;
                }
            }
        }
        reverseTail = node;
        reverseHead.next.next = reverseTail;
        reverseHead.next = pre;
        
        return fakeHead.next;
    }
}

```

# 改良算法

改良算法的可以避免最后的node引用为空边界条件，比较巧妙。

# 代码

{% highlight java %}
public ListNode reverseBetween(ListNode head, int m, int n) {
    if(head == null) return null;
    ListNode dummy = new ListNode(0); // create a dummy node to mark the head of this list
    dummy.next = head;
    ListNode pre = dummy; // make a pointer pre as a marker for the node before reversing
    for(int i = 0; i<m-1; i++) pre = pre.next;

    ListNode start = pre.next; // a pointer to the beginning of a sub-list that will be reversed
    ListNode then = start.next; // a pointer to a node that will be reversed

    // 1 - 2 -3 - 4 - 5 ; m=2; n =4 ---> pre = 1, start = 2, then = 3
    // dummy-> 1 -> 2 -> 3 -> 4 -> 5

    for(int i=0; i<n-m; i++)
    {
        start.next = then.next;
        then.next = pre.next;
        pre.next = then;
        then = start.next;
    }

    // first reversing : dummy->1 - 3 - 2 - 4 - 5; pre = 1, start = 2, then = 4
    // second reversing: dummy->1 - 4 - 3 - 2 - 5; pre = 1, start = 2, then = 5 (finish)

    return dummy.next;

}
{% endhighlight %}

# 二刷

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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode from = fakeHead;
        ListNode to = head;
        for (int i = 1; i < m; i++) {
            from = from.next;
            to = to.next;
        }
        
        ListNode iter = to.next;
        int counter = m + 1;
        while (counter <= n) {
            to.next = iter.next;
            iter.next = from.next;
            from.next = iter;
            
            iter = to.next;
            counter++;
        }
        return fakeHead.next;
    }
}
{% endhighlight %}