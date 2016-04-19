---
layout: post
title: Leetcode Problem 225 Summary
date: 2016-04-19
categories: blog
tags: [study]

---

#题目

使用Queue数据结构实现Stack数据结构

#算法

使用一个Queue，每次push操作依次将首部的元素放到尾部，直到新添加的元素到首部。其它操作不变。

#代码

	class MyStack {
    	private Queue<Integer> queue = new LinkedList<Integer>();
    
    	// Push element x onto stack.
    	public void push(int x) {
        	queue.offer(new Integer(x));
        	for (int i = 0; i < queue.size() - 1; i++) {
            	queue.offer(queue.poll());
        	}
    	}

    	// Removes the element on top of the stack.
    	public void pop() {
        	queue.poll();
    	}

    	// Get the top element.
    	public int top() {
        	return queue.peek().intValue();
    	}

    	// Return whether the stack is empty.
    	public boolean empty() {
        	return queue.isEmpty();
    	}
	}