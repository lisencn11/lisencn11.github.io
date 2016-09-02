---
layout: post
title: Leetcode Problem 146 Summary
date: 2016-09-01
categories: blog
tags: [study]

---

# 题目

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and set.

**get(key)** - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.  
**set(key, value)** - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

# 我的算法

使用双向链表 deque 配合 HashMap ，以便在常数时间内实现对象的添加和删除。

# 代码

{% highlight java %}
public class LRUCache {
    class Node {
        int key;
        int value;
        Node pre;
        Node next;
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    Node head;
    Node tail;
    int count;
    int capacity;
    Map<Integer, Node> map;
    
    private void moveHead(Node node) {
        Node pre = node.pre;
        Node next = node.next;
        pre.next = next;
        next.pre = pre;
        
        addHead(node);
    }
    
    private void addHead(Node node) {
        Node next = head.next;
        node.pre = head;
        head.next = node;
        next.pre = node;
        node.next = next;
    }
    
    public LRUCache(int capacity) {
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.pre = head;
        head.pre = null;
        tail.next = null;
        count = 0;
        this.capacity = capacity;
        map = new HashMap<>();
    }
    
    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        moveHead(node);
        return node.value;
    }
    
    public void set(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            moveHead(node);
        } else {
            Node node = new Node(key, value);
            map.put(key, node);
            if (count < capacity) {
                addHead(node);
                count++;
            } else {
                Node prePre = tail.pre.pre;
                Node pre = tail.pre;
                prePre.next = tail;
                tail.pre = prePre;
                addHead(node);
                map.remove(pre.key);
            }
        }
    }
}
{% endhighlight %}