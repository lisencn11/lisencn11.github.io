---
layout: post
title: Leetcode Problem 162 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

For example:

Given n = 5 and edges = [[0, 1], [0, 2], [0, 3], [1, 4]], return true.

Given n = 5 and edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]], return false.

Hint:

1. Given n = 5 and edges = [[0, 1], [1, 2], [3, 4]], what should your return? Is this case a valid tree?
2. According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”

# 我的算法

使用并查集算法，我犯了两个错误：  
1. 两个不同并查集的点合并时应该用根节点而非当前节点
2. 最后不需要判断是否全部节点都属于同意并查集，通过判断 edges 集合的长度和 n - 1 是否相等即可。因为对 n 个点 n - 1 次无重复连接操作必定属于同一棵树。

# 代码

{% highlight java %}
public class Solution {
    public boolean validTree(int n, int[][] edges) {
        int[] root = new int[n];
        for (int i = 0; i < n; i++) root[i] = i;
        
        for (int i = 0; i < edges.length; i++) {
            int node1 = edges[i][0];
            int node2 = edges[i][1];
            if (find(root, node1) == find(root, node2)) return false;
            
            root[find(root, node1)] = find(root, node2);
        }
        
        return edges.length == n - 1;
    }
    
    private int find(int[] root, int node) {
        while (root[node] != node) {
            root[node] = root[root[node]];
            node = root[node];
        }
        return node;
    }
}
{% endhighlight %}