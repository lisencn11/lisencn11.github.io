---
layout: post
title: Leetcode Problem 210 Summary
date: 2016-07-15
categories: blog
tags: [study]

---

# 题目

**输入**课程数量 n ，课程依次编号从 0 到 n - 1 ；课程先修要求数组 int[][] prerequisites [[0, 1]]表示 1 是 0 的先修课程。

**输出**如果能够修完全部课程则返回值为 int[] ，里面按修课顺序存储课程编号，否则返回空数组（长度为0的数组，而不是null）。

如：  
>2, [[1,0]]  
>There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1]

>4, [[1,0],[2,0],[3,1],[3,2]]  
There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is[0,2,1,3].

# 我的算法

1. 将数组表示的先修关系转换为HashMap存储的关系
2. 将入度为 0 的课程节点入队列
3. 对于队列中的节点，依次出队列并进入另一个修课顺序队列，将以其为先修课程的所有后续课程的入度减 1 ，删除先修关系，如果后续课程的入度变为 0 则将其加入队列
4. 入度为 0 的队列为空后，判断修课顺序队列长度是否等于课程数量，等于代表队列内顺序即为结果，不等于表示无法修完全部课程

# 代码

{% highlight java %}
public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        if (numCourses < 1) {
            return null;
        }
        int[] ret = null;
        if (prerequisites == null || prerequisites.length == 0 || prerequisites[0].length == 0) {
            ret = new int[numCourses];
            for (int i = 0; i < ret.length; i++)
                ret[i] = i;
            return ret;
        }

        Map<Integer, List<Integer>> afterCourse = new HashMap<>();
        Queue<Integer> zeroIn = new LinkedList<>();
        Queue<Integer> order = new LinkedList<>();
        
        int[] indegree = new int[numCourses];
        for (int i = 0; i< numCourses; i++)
            indegree[i] = 0;
        
        for (int[] prerequisite : prerequisites) {
            if (!afterCourse.containsKey(prerequisite[1]))
                afterCourse.put(prerequisite[1], new ArrayList<>());
            List<Integer> afterList = afterCourse.get(prerequisite[1]);
            afterList.add(prerequisite[0]);
            afterCourse.put(prerequisite[1], afterList);
            indegree[prerequisite[0]]++;
        }
        
        for (int i = 0; i < numCourses; i++)
            if (indegree[i] == 0) zeroIn.offer(i);
            
        while (!zeroIn.isEmpty()) {
            Integer source = zeroIn.poll();
            order.offer(source);
            if (!afterCourse.containsKey(source)) continue;
            List<Integer> afterList = afterCourse.get(source);
            for (int i = afterList.size() - 1; i >= 0; i--) {
                indegree[afterList.get(i)]--;
                if (indegree[afterList.get(i)] == 0) zeroIn.offer(afterList.get(i));
                afterList.remove(i);
            }
        }
        
        if (order.size() != numCourses) return new int[0];
        ret = new int[order.size()];
        for (int i = 0; i< ret.length; i++) 
            ret[i] = order.poll();
        return ret;
    }
}
{% endhighlight %}