---
layout: post
title: Leetcode Problem 133 Summary
date: 2016-07-11
categories: blog
tags: [study]

---

# 题目

**输入**一个无向图，每个节点包含一个整型的label域和一个包含相邻节点的List。

**输出**复制这个无向图并输出

# 我的算法

广度优先搜索，遍历到每一个节点的时候连接其与其相邻节点。

# 代码

{% highlight java %}
public class Solution {
    private HashMap<Integer, UndirectedGraphNode> map = new HashMap<>();
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        return clone(node);
    }

    private UndirectedGraphNode clone(UndirectedGraphNode node) {
        if (node == null) return null;
        
        if (map.containsKey(node.label)) {
            return map.get(node.label);
        }
        UndirectedGraphNode clone = new UndirectedGraphNode(node.label);
        map.put(clone.label, clone);
        for (UndirectedGraphNode neighbor : node.neighbors) {
            clone.neighbors.add(clone(neighbor));
        }
        return clone;
    }
}
{% endhighlight %}