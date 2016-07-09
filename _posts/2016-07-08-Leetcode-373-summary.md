---
layout: post
title: Leetcode Problem 373 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**输入**两个升序排列的整型数组nums1和nums2，一个整型k。

**输出**定义(u, v)，其中u来自nums1，v来自nums2，整型对的大小取决于u和v的和，输出前k小的整型对。

例如  
>Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

>Return: [1,2],[1,4],[1,6]

>The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

>Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

>Return: [1,1],[1,1]

>The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

>Given nums1 = [1,2], nums2 = [3],  k = 3 

>Return: [1,3],[2,3]

>All possible pairs are returned from the sequence:
[1,3],[2,3]

# 我的算法

使用优先队列（基于堆排序）PriorityQueue。随着加入整型对，优先队列能够保持从小到大排列，然后输出前k个即可。

# 代码

{% highlight java %}
public class Solution {
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<int[]> ret = new ArrayList<int[]>();
        if (nums1 == null || nums2 == null || k < 1 || nums1.length < 1 || nums2.length < 1) {
            return ret;
        }
        
        Comparator<int[]> comp = new Comparator<int[]>() {
            public int compare(int[] o1, int[] o2) {
                if ((o1[0] + o1[1]) < (o2[0] + o2[1])) {
                    return -1;
                } else if ((o1[0] + o1[1]) == (o2[0] + o2[1])) {
                    return 0;
                } else {
                    return 1;
                }
            }
        };
        
        Queue<int[]> queue = new PriorityQueue<int[]>(comp);
        for (int i = 0; i < nums1.length; i++) {
            for (int j = 0; j < nums2.length; j++) {
                int[] pair = new int[2];
                pair[0] = nums1[i];
                pair[1] = nums2[j];
                queue.offer(pair);
            }
        }
        
        for (int i = 0; i < k && !queue.isEmpty(); i++) {
            ret.add(queue.poll());
        }
        return ret;
    }
}
{% endhighlight %}