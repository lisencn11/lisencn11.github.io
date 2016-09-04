---
layout: post
title: Leetcode Problem 310 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

For a undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

Format  
The graph contains n nodes which are labeled from 0 to n - 1. You will be given the number n and a list of undirected edges (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

Example 1:

Given n = 4, edges = [[1, 0], [1, 2], [1, 3]]

return [1]

Example 2:

Given n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

return [3, 4]

Hint:

How many MHTs can a graph have at most?
Note:

(1) According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”

(2) The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

# 我的算法

对于一个无向图，如果一个节点的度为1，则在最小高度树中，其一定是作为叶子节点（特例除外）。我们发现当我们删除最外一层的叶子节点后，产生的最小高度树与原来的肯定有相同的根节点，这表示我们通过删除最外围根节点将原问题缩小了。最终我们会剩下一个节点或者一对节点，这就是我们的返回值。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) {
            List<Integer> oneNode = new ArrayList<Integer>();
            oneNode.add(0);
            return oneNode;
        }
        
        List<Set<Integer>> adjacents = new ArrayList<Set<Integer>>();
        List<Integer> leaves = new ArrayList<Integer>();
        Set<Integer> adjacent;
        int leavesLeft = n;
        
        for (int i = 0; i < n; i++) {
            adjacents.add(new HashSet<Integer>());
        }
        
        for (int[] edge : edges) {
            adjacents.get(edge[0]).add(edge[1]);
            adjacents.get(edge[1]).add(edge[0]);
        }
        
        for (int i = 0; i < n; i++) {
            if (adjacents.get(i).size() == 1) {
                leaves.add(i);
            }
        }
        
        while (leavesLeft > 2) {
            leavesLeft -= leaves.size();
            List<Integer> newLeaves = new ArrayList<Integer>();
            for (Integer leaf : leaves) {
                Integer parent = adjacents.get(leaf).iterator().next();
                adjacents.get(parent).remove(leaf);
                if (adjacents.get(parent).size() == 1) {
                    newLeaves.add(parent);
                }
            }
            leaves = newLeaves;
        }
        return leaves;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<List<Integer>> connect = new ArrayList<>();
        for (int i = 0; i < n; i++) connect.add(new ArrayList<>());
        
        for (int[] edge : edges) {
            connect.get(edge[0]).add(edge[1]);
            connect.get(edge[1]).add(edge[0]);
        }
        
        Set<Integer> set = new HashSet<>();
        int first = -1;
        int second = -1;
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        set.add(0);
        while (!queue.isEmpty()) {
            Integer node = queue.poll();
            first = node;
            List<Integer> adjacent = connect.get(node);
            for (Integer neighbor : adjacent) {
                if (!set.contains(neighbor)) {
                    set.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
        
        queue.offer(first);
        set = new HashSet<>();
        set.add(first);
        int[] pre = new int[n];
        while (!queue.isEmpty()) {
            Integer node = queue.poll();
            second = node;
            List<Integer> adjacent = connect.get(node);
            for (Integer neighbor : adjacent) {
                if (!set.contains(neighbor)) {
                    set.add(neighbor);
                    queue.offer(neighbor);
                    pre[neighbor] = node;
                }
            }
        }
        
        List<Integer> list = new ArrayList<>();
        int iter = second;
        while (iter != first) {
            list.add(iter);
            iter = pre[iter];
        }
        list.add(iter);
        int half = list.size() / 2;
        List<Integer> ret = new ArrayList<>();
        ret.add(list.get(half));
        if (list.size() % 2 == 0) ret.add(list.get(half - 1));
        return ret;
    }
}
{% endhighlight %}