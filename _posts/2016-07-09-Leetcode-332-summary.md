---
layout: post
title: Leetcode Problem 332 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

机票，[JFK, SFO]表示从JFK机场到SFO机场。

**输入**一个数组，包含很多张机票信息。

**输出**根据机票生成的经过的机场的排序，如果存在多种排序可能，按字典序进行排列。

如：  
>tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]  

>Return ["JFK","ATL","JFK","SFO","ATL","SFO"].  

>Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"]. But it is larger in lexical order.

# 我的算法

首先对数据结构进行整理，使用HashMap<String, PriorityQueue<String>>可以更加快速的对机票进行字典序排序和查找。

对于生成行程，进行考虑到前提是一定可以形成一个有效的行程，那么图中可能存在多个环，进过多次JFK，但是最多只有一个机场能够终止，即断头路，所以使用栈Stack对机场进行存储，当发现无法继续飞行的时候即表示找到终点，然后倒序输入到List中。

# 代码

{% highlight java %}
public class Solution {
    public List<String> findItinerary(String[][] tickets) {
        Map<String, Queue<String>> map = new HashMap<>();
        List<String> ret = new ArrayList<>();
        String from = null;
        String to = null;
        Stack<String> stack = new Stack<>();
        
        for (int i = 0; i < tickets.length; i++) {
            from = tickets[i][0];
            to = tickets[i][1];
            if (!map.containsKey(from)) {
                Queue<String> newQueue = new PriorityQueue<>();
                map.put(from, newQueue);
            }
            Queue<String> queue = map.get(from);
            queue.offer(to);
            map.put(from, queue);
        }
        
        from = "JFK";
        stack.push(from);
        while (!stack.isEmpty()) {
            from = stack.peek();
            while (map.containsKey(from) && !map.get(from).isEmpty()) {
                Queue<String> queue = map.get(from);
                to = queue.poll();
                stack.push(to);
                from = to;
            }
            to = stack.pop();
            ret.add(0, to);
        }
        return ret;
    }
}
{% endhighlight %}