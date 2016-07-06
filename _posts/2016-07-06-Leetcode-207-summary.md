---
layout: post
title: Leetcode Problem 207 Summary
date: 2016-07-06
categories: blog
tags: [study]

---

# 题目

**输入**课程数量n，先修依赖条件int[][] prerequisites，[[0, 1],[1,2],...]表示修编号为0的课程需要先修编号为1的课程。

**输出**先修依赖条件是否满足能够修完所有课程。

例子：  
2, [[1, 0]] true  
2, [[1, 0], [0, 1]] false

# 我的算法

如果不能修完所有课程意味着课程以来存在环依赖，即有些课程永远无法修，将课程看成一个个节点的话，此题等同于设计算法检测图里是否存在环。

最开始**错误**的算法是，使用广度优先搜索，看是否遍历到重复节点，错误的原因是有向图存在多父母的情况，此时会重复遍历但不是环。如：[[0, 1], [0, 2], [1, 2]]。

这里不详细介绍检测图中是否含有环的算法，下面代码使用了[TenosDoIt博客](http://www.cnblogs.com/TenosDoIt/p/3644225.html)中的方法3，通过不断删除入度为0的节点和其出发的边，最后如果含有环的话则节点一定删除不完，因为环中的节点和边永远不会被删除。

# 代码

{% highlight java %}
public class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> sourceList = new ArrayList<List<Integer>>();
        List<Integer> sourceNode = null;
        Stack<Integer> zeroInDegree = new Stack<Integer>();
        int[] inDegree = new int[numCourses];
        Set<Integer> set = new HashSet<Integer>();
        int nodeCounter = 0;
        
        int x = 0;
        int y = 0;
        
        for (int i = 0; i < numCourses; i++) {
            inDegree[i] = 0;
        }
        
        for (int i = 0; i < numCourses; i++) {
            sourceNode = new ArrayList<Integer>();
            sourceList.add(sourceNode);
        }
        
        for (int i = 0; i < prerequisites.length; i++) {
            x = prerequisites[i][0];
            y = prerequisites[i][1];
            sourceNode = sourceList.get(x);
            sourceNode.add(y);
            inDegree[y]++;
        }
        
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                zeroInDegree.push(i);
            }
        }
        
        while (!zeroInDegree.isEmpty()) {
            Integer source = zeroInDegree.pop();
            nodeCounter++;
            sourceNode = sourceList.get(source);
            for (Integer dest : sourceNode) {
                inDegree[dest]--;
                if (inDegree[dest] == 0) {
                    zeroInDegree.push(dest);
                }
            }
            sourceNode.clear();
        }
        
        if (nodeCounter != numCourses) {
            return false;
        }
        
        return true;
    }
}
{% endhighlight %}