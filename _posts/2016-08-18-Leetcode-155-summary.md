---
layout: post
title: Leetcode Problem 155 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* getMin() -- Retrieve the minimum element in the stack.

Example:  
MinStack minStack = new MinStack();  
minStack.push(-2);  
minStack.push(0);  
minStack.push(-3);  
minStack.getMin();   --> Returns -3.  
minStack.pop();  
minStack.top();      --> Returns 0.  
minStack.getMin();   --> Returns -2.

# 我的算法

使用 TreeMap 红黑树来实现 O(logn) 时间内的查找最小值操作。

discuss 中有许多好方法：

1. 栈中存储差值
2. 入栈时加入标记位表示 min 被替换

# 代码

{% highlight java %}
public class MinStack {
    Stack<Integer> stack;
    TreeMap<Integer, Integer> tree;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack();
        tree = new TreeMap<>();
    }
    
    public void push(int x) {
        stack.push(x);
        if (!tree.containsKey(x)) {
            tree.put(x, 1);
        } else {
            tree.put(x, tree.get(x) + 1);
        }
    }
    
    public void pop() {
        Integer i = stack.pop();
        if (tree.get(i) == 1) {
            tree.remove(i);
        } else {
            tree.put(i, tree.get(i) - 1);
        }
    }
    
    public int top() {
        return (int)stack.peek();
    }
    
    public int getMin() {
        return (int)tree.firstKey();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
{% endhighlight %}

# discuss

存储差值

{% highlight java %}
public class MinStack {
    Stack<Long> stack;
    long min;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
    }
    
    public void push(int x) {
        if (stack.isEmpty()) {
            stack.push(0L);
            min = (long)x;
        } else {
            stack.push(x - min);
            min = x < min ? (long)x : min;
        }
    }
    
    public void pop() {
        if (stack.isEmpty()) return;
        
        long pop = stack.pop();
        
        if (pop < 0) min = min - pop;
    }
    
    public int top() {
        long top = stack.peek();
        
        if (top > 0) return (int)(min + top);
        else return (int)min;
    }
    
    public int getMin() {
        return (int)min;
    }
}
{% endhighlight %}

入栈时加入标记位表示 min 被替换

{% highlight java %}
public class MinStack {
    Stack<Integer> stack;
    int min;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        min = Integer.MAX_VALUE;
    }
    
    public void push(int x) {
        if (x <= min) {
            stack.push(min);
            min = x;
        }
        stack.push(x);
    }
    
    public void pop() {
        if (stack.peek() == min) {
            stack.pop();
            min = stack.peek();
            stack.pop();
        } else {
            stack.pop();
        }
        
        if (stack.isEmpty()) min = Integer.MAX_VALUE;
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return min;
    }
}
{% endhighlight %}