---
layout: post
title: Leetcode Problem 347 Summary
date: 2016-06-07
categories: blog
tags: [study]

---

# 题目

输入一个整型数组，统计每个元素出现的个数，然后返回前k多的元素的List。要求时间复杂度好于O(nlogn)。


# 算法

可以使用HashMap，在O(n)时间复杂度的情况下统计元素出现次数，然后使用桶排序，以出现次数作为下标，元素值作为存储的value。初始化一个List数组，用于存储出现次数相同的元素。

# 代码

```java

public class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> numberTimesMap = new HashMap<Integer, Integer>();
        List<Integer>[] buckets = new List[nums.length + 1];

        for (int n : nums) {
            numberTimesMap.put(n, numberTimesMap.getOrDefault(n, 0) + 1);
        }
        
        for (int key : numberTimesMap.keySet()) {
            int value = numberTimesMap.get(key);
            if (buckets[value] == null) {
                buckets[value] = new ArrayList<Integer>();
            }
            buckets[value].add(key);
        }
        
        List<Integer> res = new ArrayList<Integer>();
        
        for (int i = buckets.length - 1; i >= 0 && res.size() < k; i--) {
            if (buckets[i] != null) {
                res.addAll(buckets[i]);
            }
        }
        
        return res;
    }
}

```

