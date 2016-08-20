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

{% highlight java %}
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
{% endhighlight %}

# 二刷的做法

{% highlight java %}
public class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> numToTimes = new HashMap<>();
        List<Pair> list = new ArrayList<>();
        int iter = 0;
        int max = 1;
        
        for (int i = 0; i < nums.length; i++) {
            if (numToTimes.containsKey(nums[i])) {
                int index = numToTimes.get(nums[i]);
                Pair pair = list.get(index);
                pair.count++;
                max = pair.count > max ? pair.count : max;
            } else {
                numToTimes.put(nums[i], iter++);
                Pair pair = new Pair(nums[i], 1);
                list.add(pair);
            }
        }
        
        List<List<Pair>> buckets = new ArrayList<>();
        for (int i = 0; i < max; i++) {
            buckets.add(new ArrayList<>());
        }
        for (int i = 0; i < list.size(); i++) {
            Pair pair = list.get(i);
            buckets.get(pair.count - 1).add(pair);
        }
        
        List<Integer> ret = new ArrayList<>();
        for (int i = max - 1; i >= 0; i--) {
            List<Pair> temp = buckets.get(i);
            if (!temp.isEmpty()) {
                for (int j = 0; j < temp.size(); j++) {
                    ret.add(temp.get(j).num);
                    k--;
                    if (k == 0) return ret;
                }
            }
        }
        return ret;
    }
    
    class Pair {
        int num;
        int count;
        
        Pair(int num, int count) {
            this.num = num;
            this.count = count;
        }
    }
}
{% endhighlight %}

