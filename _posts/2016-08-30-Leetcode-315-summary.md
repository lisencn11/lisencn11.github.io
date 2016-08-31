---
layout: post
title: Leetcode Problem 315 Summary
date: 2016-08-30
categories: blog
tags: [study]

---

# 题目

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

Example:

>Given nums = [5, 2, 6, 1]

>To the right of 5 there are 2 smaller elements (2 and 1).  
>To the right of 2 there is only 1 smaller element (1).  
>To the right of 6 there is 1 smaller element (1).  
>To the right of 1 there is 0 smaller element.

Return the array [2, 1, 1, 0].

# 我的算法

利用 BST 对数据进行存储，结点中除了 value 域，还很巧妙的加入了 less 域和 dup 域。 less域记录当前有多少个小于这个结点的元素，dup 记录有多少个重复的这个元素，当我们将一个结点插入这棵树中的时候可以利用这两个域计算出当前有多少元素小于当前元素。

# 代码

{% highlight java %}
public class Solution {
    class Node {
        Node left, right;
        int less, dup, val;
        public Node(int val) {
            this.left = null;
            this.right = null;
            this.less = 0;
            this.dup = 1;
            this.val = val;
        }
    }
    
    private void insert(int[] nums, int[] counts, int index, Node root) {
        Node iter = root;
        int count = 0, value = nums[index];
        Node pre = null;
        while (iter != null) {
            pre = iter;
            if (value > iter.val) {
                count += iter.less + iter.dup;
                iter = iter.right;
            } else if (value < iter.val) {
                iter.less++;
                iter = iter.left;
            } else {
                iter.dup++;
                count += iter.less;
                counts[index] = count;
                return;
            }
        }
        
        Node curr = new Node(value);
        if (pre.val < value) {
            pre.right = curr;
        } else {
            pre.left = curr;
        }
        counts[index] = count;
    }
    
    public List<Integer> countSmaller(int[] nums) {
        if (nums == null || nums.length == 0) return new ArrayList<>();
        int len = nums.length;
        int[] counts = new int[len];
        Node root = new Node(nums[len - 1]);
        counts[len - 1] = 0;
        
        for (int i = len - 2; i >= 0; i--) {
            insert(nums, counts, i, root);
        }
        
        List<Integer> ret = new ArrayList<>();
        for (int i : counts) {
            ret.add(i);
        }
        return ret;
    }
}
{% endhighlight %}