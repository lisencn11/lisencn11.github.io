---
layout: post
title: Leetcode Problem 18 Summary
date: 2016-07-11
categories: blog
tags: [study]

---

# 题目

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:  
[  
  [-1,  0, 0, 1],  
  [-2, -1, 1, 2],  
  [-2,  0, 0, 2]  
]

# 我的算法

对于a和d这两个两头元素，遍历所有可能的组合，时间复杂度为O(n<sup>2</sup>)，而对于b和c这两个元素，采用从两头到中间的遍历方法O(n)。故总的时间复杂度为O(n<sup>3</sup>)。

# 代码

{% highlight java %}
public class Solution {
    Set<List<Integer>> set = null;
    
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> ret = new ArrayList<>();
        if (nums == null || nums.length < 4) {
            return ret;
        }
        
        Arrays.sort(nums);
        set = new HashSet<>();
        for (int a = 0, d = nums.length - 1; ((a + 3) <= d); a++, d--) {
            int newA = a, newD = d;
            while (newA + 3 <= d) {
                addToList(nums, newA, d, target);
                newA++;
            }
            while (a + 3 <= newD) {
                addToList(nums, a, newD, target);
                newD--;
            }
        }
        
        ret = new ArrayList<>(set);
        return ret;
    }
    
    private void addToList(int[] nums, int a, int d, int target) {
        int b = a + 1, c = d - 1;
        int sum = nums[a] + nums[d];
        while (b < c) {
            if (sum + nums[b] + nums[c] == target) {
                List<Integer> list = new ArrayList<>();
                list.add(nums[a]);
                list.add(nums[b]);
                list.add(nums[c]);
                list.add(nums[d]);
                set.add(list);
                if ((nums[b + 1] - nums[b]) < (nums[c] - nums[c - 1])) b++;
                else c--;
            } else if (sum + nums[b] + nums[c] < target) {
                b++;
            } else {
                c--;
            }
        }
    }
}
{% endhighlight %}

# 改进

可以通过加上一些限定条件明显改善时间复杂度，如：

* 当前上界nums[d] * 4 < target，则可直接返回结果
* 当前下界nums[a] * 4 > target，则可直接返回结果
* 当前nums[a] == nums[a + 1]或者nums[d] == nums[d - 1]，则可以跳过下个元素，因为最后不能含有相同结果

# 二刷

{% highlight java %}
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) return new ArrayList<>();
        
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0; i < nums.length - 3; i++) {
            for (int j = i + 1; j < nums.length - 2; j++) {
                int p1 = j + 1;
                int p2 = nums.length - 1;
                int preSum = nums[i] + nums[j];
                while (p1 < p2) {
                    if (preSum + nums[p1] + nums[p2] > target) {
                        int oldValue = nums[p2];
                        while (p2 > p1 && nums[p2] == oldValue) {
                            p2--;
                        }
                    } else if (preSum + nums[p1] + nums[p2] < target) {
                        int oldValue = nums[p1];
                        while (p1 < p2 && nums[p1] == oldValue) {
                            p1++;
                        }
                    } else {
                        List<Integer> list = new ArrayList<>();
                        list.add(nums[i]);
                        list.add(nums[j]);
                        list.add(nums[p1]);
                        list.add(nums[p2]);
                        result.add(list);
                        int oldValue = nums[p1];
                        while (p1 < p2 && nums[p1] == oldValue) {
                            p1++;
                        }
                        oldValue = nums[p2];
                        while (p1 < p2 && nums[p2] == oldValue) {
                            p2--;
                        }
                    }
                }
                int oldValue = nums[j];
                while (j < nums.length - 2 && nums[j] == oldValue) {
                    j++;
                }
                j--;
            }
            int oldValue = nums[i];
            while (i < nums.length - 3 && nums[i] == oldValue) {
                i++;
            }
            i--;
        }
        return result;
    }
}
{% endhighlight %}