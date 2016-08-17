---
layout: post
title: Leetcode Problem 225 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Implement the following operations of a stack using queues.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* empty() -- Return whether the stack is empty.
* 
Notes:

You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.

Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.

You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

# 我的算法

每次 push 操作都将之前的 n - 1 个元素进行 queue.offer(poll()) 实现后进先出。

# 代码

{% highlight java %}
class MyStack {
    Queue<Integer> queue = new LinkedList<>();
    
    // Push element x onto stack.
    public void push(int x) {
        int len = queue.size();
        queue.offer(x);
        for (int i = 0; i < len; i++) queue.offer(queue.poll());
    }

    // Removes the element on top of the stack.
    public void pop() {
        queue.poll();
    }

    // Get the top element.
    public int top() {
        return queue.peek();
    }

    // Return whether the stack is empty.
    public boolean empty() {
        return queue.isEmpty();
    }
}
{% endhighlight %}