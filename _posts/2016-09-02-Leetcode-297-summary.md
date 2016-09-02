---
layout: post
title: Leetcode Problem 297 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

For example, you may serialize the following tree

![](https://lisencn11.github.io/img/problem297.png)

as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

# 我的算法

用先序遍历进行序列化成一个 String ，反序列化也是先序，注意两点：  
1. 结点 value 应该用 Integer.valueOf() 来反向获得  
2. 用 , 来分割每个结点

以上两个问题出现的原因是我没有考虑到 value 取负值和两位数以上的情况。

# 代码

{% highlight java %}
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder preSb = new StringBuilder();
        dfs(root, preSb);
        return preSb.toString();
    }
    
    private void dfs(TreeNode root, StringBuilder preSb) {
        if (root == null) {
            preSb.append("n").append(",");
            return;
        }
        preSb.append(root.val).append(",");
        dfs(root.left, preSb);
        dfs(root.right, preSb);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        Queue<String> queue = new LinkedList<>();
        for (String s : nodes) queue.offer(s);
        TreeNode root = dfs1(queue);
        return root;
    }
    
    private TreeNode dfs1(Queue<String> queue) {
        String val = queue.poll();
        if (val.equals("n")) return null;
        else {
            TreeNode node = new TreeNode(Integer.valueOf(val));
            node.left = dfs1(queue);
            node.right = dfs1(queue);
            return node;
        }
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
{% endhighlight %}