---
layout: post
title: Leetcode Problem 325 Summary
date: 2016-08-22
categories: blog
tags: [study]

---

# 题目

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Example 1:  
Given nums = [1, -1, 5, -2, 3], k = 3,  
return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

Example 2:  
Given nums = [-2, -1, 2, 1], k = 1,  
return 2. (because the subarray [-1, 2] sums to 1 and is the longest)

Follow Up:  
Can you do it in O(n) time?

# 我的算法

刚开始考虑了 DP 和 two pointer，其实 DP 是可以的但是没有想起哈希表这个数据结构导致排除了 DP 算法。。。

简单来说我们使用哈希表， key 为从头开始到 index 为 i 的加和，我们知道对于 i 到 j 的加和可以用 sum[j] - sum[i - 1] 求得。于是我们哈希表中的 value 保存着最小的对应加和为 key 的 index。这样在遍历数组时，我们 check 是否有 sum - target 的 key 的键值对，如果有表明存在子序列和为 target，接下来只需要计算出长度再和当前的最大长度进行比较即可。

# 代码

{% highlight java %}
public class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        map.put(sum, -1);
        int len = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k)) {
                int pre = map.get(sum - k);
                len = (i - pre) > len ? (i - pre) : len;
            }
            if (!map.containsKey(sum)) map.put(sum, i);
        }
        
        return len;
    }
}
{% endhighlight %}