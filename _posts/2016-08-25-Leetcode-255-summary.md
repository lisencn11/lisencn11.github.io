---
layout: post
title: Leetcode Problem 255 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

Follow up:  
Could you do it using only constant space complexity?

# 我的算法

最开始的算法是简单的 backtrack ，先遍历知道遇到一个大于 root 的结点，然后去 check 之后是否也一直大于 root，然后再去 check 左右子树。时间复杂度，左右子树是 O(logn)，遍历是O(n)，所以总共是 O(nlogn)。

改善效率：考虑是否有 O(n) 的算法，用一个栈记录当前上界，当遇到更大的数时，探出栈作为下界，当遇到出界的数时返回 false。

# 代码

backtrack:

{% highlight java %}
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        return helper(preorder, 0, preorder.length - 1);
    }
    
    private boolean helper(int[] preorder, int start, int end) {
        if (start >= end) return true;
        
        int root = preorder[start];
        int right = start + 1;
        while (right <= end) {
            if (preorder[right] < root) right++;
            else break;
        }
        
        int check = right;
        while (check <= end) {
            if (preorder[check] < root) return false;
            check++;
        }
        
        return helper(preorder, start + 1, right - 1) && helper(preorder, right, end);
    }
}
{% endhighlight %}

Stack:

{% highlight java %}
public class Solution {
    public boolean verifyPreorder(int[] preorder) {
        if (preorder == null || preorder.length == 0) return true;
        Stack<Integer> stack = new Stack<>();
        stack.push(preorder[0]);
        int low = Integer.MIN_VALUE;
        for (int i = 1; i < preorder.length; i++) {
            if (preorder[i] < stack.peek() && preorder[i] >= low) {
                stack.push(preorder[i]);
            } else if (preorder[i] > stack.peek()){
                while (!stack.isEmpty() && preorder[i] > stack.peek()) {
                    low = stack.pop();
                }
                stack.push(preorder[i]);
            } else if (preorder[i] < low) {
                return false;
            }
        }
        
        return true;
    }
}
{% endhighlight %}

# discuss

follow up 是要求 O(1) 空间复杂度，discuss 给出的解是利用 preorder 数组做栈。

{% highlight java %}
public boolean verifyPreorder(int[] preorder) {
    int low = Integer.MIN_VALUE, i = -1;
    for (int p : preorder) {
        if (p < low)
            return false;
        while (i >= 0 && p > preorder[i])
            low = preorder[i--];
        preorder[++i] = p;
    }
    return true;
}
{% endhighlight %}