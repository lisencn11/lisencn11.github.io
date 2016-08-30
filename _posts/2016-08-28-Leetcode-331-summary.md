---
layout: post
title: Leetcode Problem 331 Summary
date: 2016-08-28
categories: blog
tags: [study]

---

# 题目

One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as #.

![](https://lisencn11.github.io/img/problem331.png)

For example, the above binary tree can be serialized to the string "9,3,4,#,#,1,#,#,2,#,6,#,#", where # represents a null node.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character '#' representing null pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as "1,,3".

Example 1:  
"9,3,4,#,#,1,#,#,2,#,6,#,#"  
Return true

Example 2:  
"1,#"  
Return false

Example 3:  
"9,#,#,1"  
Return false

# 我的算法

用栈，遇到两个连续 # 时将这两个 # 连同之前的元素弹出，再压入一个 # 。

用 degree，一个树中的出度和入度相加总和为 0。

# 代码

### 栈

{% highlight java %}
public class Solution {
    public boolean isValidSerialization(String preorder) {
        if (preorder == null || preorder.length() == 0) return true;
        String[] nodes = preorder.split(",");
        Stack<String> stack = new Stack<>();
        
        for (int i = 0; i < nodes.length; i++) {
            if (nodes[i].equals("#")) {
                while (!stack.isEmpty() && stack.peek().equals("#")) {
                    stack.pop();
                    stack.pop();
                }
                if (stack.isEmpty() && i != nodes.length - 1) return false;
                stack.push("#");
            } else {
                stack.push(nodes[i]);
            }
        }
        
        return stack.size() == 1 && stack.pop().equals("#");
    }
}
{% endhighlight %}

### 出入度

{% highlight java %}
 public boolean isValidSerialization(String preorder) {
    String[] strs = preorder.split(",");
    int degree = -1;         // root has no indegree, for compensate init with -1
    for (String str: strs) {
        degree++;             // all nodes have 1 indegree (root compensated)
        if (degree > 0) {     // total degree should never exceeds 0
            return false;
        }      
        if (!str.equals("#")) {// only non-leaf node has 2 outdegree
            degree -= 2;
        }  
    }
    return degree == 0;
}
{% endhighlight %}