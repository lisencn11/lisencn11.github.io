---
layout: post
title: Leetcode Problem 323 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:

![](https://lisencn11.github.io/img/problem323_1.png)

Given n = 5 and edges = [[0, 1], [1, 2], [3, 4]], return 2.

Example 2:

![](https://lisencn11.github.io/img/problem323_2.png)

Given n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]], return 1.

Note:  
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

# 我的算法

使用了两种算法：BFS 和 并查集( Union Find )。关于并查集的介绍在[这里](http://blog.csdn.net/dm_vincent/article/details/7655764)

# 代码

### BFS

{% highlight java %}
public class Solution {
    List<List<Integer>> connect;
    Set<Integer> visited;
    
    public int countComponents(int n, int[][] edges) {
        connect = new ArrayList<>();
        visited = new HashSet<>();
        int count = 0;
        for (int i = 0; i < n; i++) {
            List<Integer> list = new ArrayList<>();
            connect.add(list);
        }
        
        for (int i = 0; i < edges.length; i++) {
            int node1 = edges[i][0];
            int node2 = edges[i][1];
            connect.get(node1).add(node2);
            connect.get(node2).add(node1);
        }
        
        for (int i = 0; i < n; i++) {
            if (!visited.contains(i)) {
                bfs(i);
                count++;
            }
        }
        
        return count;
    }
    
    private void bfs(int startNode) {
        Queue<Integer> bfsQueue = new LinkedList<>();
        bfsQueue.offer(startNode);
        visited.add(startNode);
        
        while (!bfsQueue.isEmpty()) {
            int node = bfsQueue.poll();
            List<Integer> neighbors = connect.get(node);
            for (int i = 0; i < neighbors.size(); i++) {
                if (!visited.contains(neighbors.get(i))) {
                    bfsQueue.add(neighbors.get(i));
                    visited.add(neighbors.get(i));
                }
            }
        }
    }
}
{% endhighlight %}

### Union Find

{% highlight java %}
public class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] roots = new int[n];
        for (int i = 0; i < n; i++) {
            roots[i] = i;
        }
        
        int count = n;
        for (int i = 0; i < edges.length; i++) {
            int root1 = find(roots, edges[i][0]);
            int root2 = find(roots, edges[i][1]);
            if (root1 != root2) {
                roots[root1] = root2;
                count--;
            }
        }
        
        return count;
    }
    
    private int find(int[] roots, int id) {
        while (roots[id] != id) {
            roots[id] = roots[roots[id]];
            id = roots[id];
        }
        
        return id;
    }
}
{% endhighlight %}