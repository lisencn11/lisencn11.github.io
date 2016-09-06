---
layout: post
title: Leetcode Problem 239 Summary
date: 2016-09-04
categories: blog
tags: [study]

---

# 题目

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

For example,  
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

![](https://lisencn11.github.io/img/problem239.png)

Therefore, return the max sliding window as [3,3,5,5,6,7].

Note:   
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

Follow up:  
Could you solve it in linear time?

Hint:

1. How about using a data structure such as deque (double-ended queue)?
2. The queue size need not be the same as the window’s size.
3. Remove redundant elements and the queue should store only elements that need to be considered.

# 我的算法

1. 双向链表
2. 双向链表中，头部存着至今为止的最大元素，右边存着依次递减的直到 i 的元素
3. 通过两个操作维护链表，一是扔掉左边所有过期元素，二是扔掉右边所有小于当前元素的元素

# 代码

{% highlight java %}
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k <= 0) return new int[0];
        
        ArrayDeque<Integer> deque = new ArrayDeque<>();
        int[] result = new int[nums.length - k + 1];
        int iter = 0;
        for (int i = 0; i < nums.length; i++) {
            while (!deque.isEmpty() && deque.peek() < (i - k + 1)) {
                deque.poll();
            }
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            deque.offer(i);
            if (i >= k - 1) {
                result[iter++] = nums[deque.peek()];
            }
        }
        return result;
    }
}
{% endhighlight %}