---
layout: post
title: Leetcode Problem 220 Summary
date: 2016-07-21
categories: blog
tags: [study]

---

# 题目

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the difference between nums[i] and nums[j] is at most t and the difference between i and j is at most k.

# 我的算法

如果使用暴力算法尝试每一对 index ，我们需要O(n<sup>2</sup>)的时间复杂度。

考虑到，序号（index）一定是一个接一个的，即使我们使用 k 对 index 进行剪枝，复杂度仍然是 O(n * k)，所以更好的是我们用 t 对排序后的 nums[i] 进行剪枝。这样在最坏情况下的时间复杂度没有改善，但是考虑到比起 index ， value 通常被剪枝的可能性更大，所以能做到一定的优化。

最后是一些边界条件的考虑：

1. 两个 index 有相同值的情况，对于程序中我使用的哈希表会取得同一个 List ，导致必然返回 true ，所以采取想哈希表中插入相同值时就进行检查的方法
2. 因为是整型数组，求差时可能出现整型越界情况，处理方法是使用long型数组

# 代码

{% highlight java %}
public class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (nums == null || nums.length < 2 || k < 0 || t < 0) return false;
        
        Map<Long, List<Integer>> map = new HashMap<>();
        
        long[] num = new long[nums.length];
        for (int i = 0; i < nums.length; i++)
            num[i] = (long)nums[i];
        
        for (int i = 0; i < num.length; i++) {
            if (!map.containsKey(num[i])) map.put(num[i], new ArrayList<>());
            List<Integer> list = map.get(num[i]);
            if (list.size() != 0 && (i - list.get(list.size() - 1) <= k)) return true;
            list.add(i);
            map.put(num[i], list);
        }
        
        Arrays.sort(num);
        for (int i = 0; i < num.length - 1; i++) {
            if (num[i + 1] == num[i]) continue;
            int iter = 1;
            while (i + iter < num.length && num[i + iter] - num[i] <= t) {
                List<Integer> list1 = map.get(num[i]);
                List<Integer> list2 = map.get(num[i + iter]);
                int p1 = 0, p2 = 0;
                while (p1 < list1.size() && p2 < list2.size()) {
                    if (Math.abs(list1.get(p1) - list2.get(p2)) <= k) return true;
                    if (list1.get(p1) > list2.get(p2)) p2++;
                    else p1++;
                }
                iter++;
            }
        }
        return false;
    }
}
{% endhighlight %}

# 改进

这里贴上discuss中达到O(n)时间复杂度的解法：

思路是：

因为我们需要 nums[i] 和 nums[j] 的差值最多为 t ，那么我们使用一个桶，使得 i * t + 1 到 (i + 1) * t 之间的数进入一个桶（ num / ( t + 1 ) ）即可，这样如果有两个数进入一个桶则必然发现一个满足条件的，相邻桶也可能有满足条件的，随着遍历序号的增加删除掉过期的桶即可保证序号差不超过 k 。

{% highlight java %}
public class Solution {
	public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (k < 1 || t < 0) return false;
        Map<Long, Long> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            long remappedNum = (long) nums[i] - Integer.MIN_VALUE;
            long bucket = remappedNum / ((long) t + 1);
            if (map.containsKey(bucket)
                    || (map.containsKey(bucket - 1) && remappedNum - map.get(bucket - 1) <= t)
                        || (map.containsKey(bucket + 1) && map.get(bucket + 1) - remappedNum <= t))
                            return true;
            if (map.entrySet().size() >= k) {
                long lastBucket = ((long) nums[i - k] - Integer.MIN_VALUE) / ((long) t + 1);
                map.remove(lastBucket);
            }
            map.put(bucket, remappedNum);
        }
        return false;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
         if (nums == null || nums.length == 0) return false;
         TreeSet<Integer> set = new TreeSet<>();
         
         for (int i = 0; i < nums.length; i++) {
             Integer floor = set.floor(nums[i]);
             Integer ceil = set.ceiling(nums[i]);
             if ((floor != null && floor >= nums[i] - t) || (ceil != null && ceil <= nums[i] + t)) {
                 return true;
             }
             set.add(nums[i]);
             if (i >= k) {
                 set.remove(nums[i - k]);
             }
         }
         return false;
    }
}
{% endhighlight %}