---
layout: post
title: Leetcode Problem 310 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

**输入**一个无向图，这个图可以表示成树，即图中不存在环。

**输出**找到最小高度树minimum height trees (MHTs)的根节点。

例如

n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

返回List[3, 4]，表示3或者4座位根节点都可以达到最小高度树。


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