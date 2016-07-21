---
layout: post
title: Leetcode Problem 220 Summary
date: 2016-07-21
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组 nums ，一个整型t，一个整型k。

**输出** boolean 类型，在整型数组中，是否能够找到两个不同的 index i 和 j ，满足 nums[i] 和 nums[j]差值不大于 t ，同时 i 和 j 的差值不大于 k。

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