---
layout: post
title: Leetcode Problem 15 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:  
[  
  [-1, 0, 1],  
  [-1, -1, 2]  
]

# 我的算法

选定最左元素和最右元素，利用二分查找来尝试合适的中间元素，复杂度为O(n<sup>2</sup>logn)。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        if (nums == null || nums.length < 3) return new ArrayList<>();
        
        List<List<Integer>> ret = new ArrayList<>();
        Arrays.sort(nums);

        for (int start = 0; start < (nums.length - 2); start++) {
            for (int end = nums.length - 1; start < (end - 1); end--) {
                int low = start + 1;
                int high = end - 1;
                List<Integer> list = null;
                while (low <= high) {
                    int mid = low + (high - low) / 2;
                    int sum = nums[start] + nums[end] + nums[mid];
                    if (sum > 0) {
                        high = mid - 1;
                    } else if (sum < 0) {
                        low = mid + 1;
                    } else {
                        list = new ArrayList<>();
                        list.add(nums[start]);
                        list.add(nums[mid]);
                        list.add(nums[end]);
                        break;
                    }
                }
                if (list != null) ret.add(list);
                while ((start < end - 1) && nums[end] == nums[end - 1]) end--;
            }
            while ((start < nums.length - 2) && nums[start] == nums[start + 1]) start++;
        }
        
        return ret;
    }
}
{% endhighlight %}

# 改进

更简单且复杂度更低的方法没有想到，直接选定第一个元素，然后利用两个指针从两头向中间来查找合适的值。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> threeSum(int[] num) {
        Arrays.sort(num);
        List<List<Integer>> res = new LinkedList<>(); 
        for (int i = 0; i < num.length-2; i++) {
            if (i == 0 || (i > 0 && num[i] != num[i-1])) {
                int lo = i+1, hi = num.length-1, sum = 0 - num[i];
                while (lo < hi) {
                    if (num[lo] + num[hi] == sum) {
                        res.add(Arrays.asList(num[i], num[lo], num[hi]));
                        while (lo < hi && num[lo] == num[lo+1]) lo++;
                        while (lo < hi && num[hi] == num[hi-1]) hi--;
                        lo++; hi--;
                    } else if (num[lo] + num[hi] < sum) lo++;
                    else hi--;
               }
            }
        }
        return res;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 2; i++) {
            int p1 = i + 1;
            int p2 = nums.length - 1;
            while (p1 < p2) {
                if (nums[i] + nums[p1] + nums[p2] == 0) {
                    List<Integer> list = new ArrayList<>();
                    list.add(nums[i]);
                    list.add(nums[p1]);
                    list.add(nums[p2]);
                    result.add(list);
                    p1++;
                    p2--;
                    while (p1 < p2 && nums[p1] == nums[p1 - 1]) p1++;
                    while (p1 < p2 && nums[p2] == nums[p2 + 1]) p2--;
                } else if (nums[i] + nums[p1] + nums[p2] < 0) {
                    p1++;
                    while (p1 < p2 && nums[p1] == nums[p1 - 1]) p1++;
                } else {
                    p2--;
                    while (p1 < p2 && nums[p2] == nums[p2 + 1]) p2--;
                }
            }
            while (i < nums.length - 1 && nums[i] == nums[i + 1]) i++;
        }
        return result;
    }
}
{% endhighlight %}