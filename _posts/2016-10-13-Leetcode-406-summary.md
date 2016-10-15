---
layout: post
title: Leetcode Problem 406 Summary
date: 2016-10-13
categories: blog
tags: [study]

---

# 题目

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:  
The number of people is less than 1,100.

Example

Input:  
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:  
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]

# 我的算法

首先对people数组排序，第一排序规则是h从大到小，对于相同大小的h，按k从小到大排序。这样对于第i个people，他前面的i-1个people的身高必定大于等于他，只需要把他插入到第people[1]个位置即满足条件。

# 代码

{% highlight java %}
public class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if (people == null || people.length == 0 || people[0].length == 0) return people;
        
        int len = people.length;
        List<int[]> list = new ArrayList<>();

        Arrays.sort(people, new Comparator<int[]>(){
            public int compare(int[] o1, int[] o2) {
                if (o1[0] != o2[0]) return o2[0] - o1[0];
                else return o1[1] - o2[1];
            }
        });
        
        for (int[] person : people) {
            list.add(person[1], person);
        }
        
        return list.toArray(new int[len][people[0].length]);
    }
}
{% endhighlight %}