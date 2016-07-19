---
layout: post
title: Leetcode Problem 15 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组

**输出**一个List<List<Integer>>，其中每个List由数组中的三个元素组成，这三个元素加和为 0 。

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