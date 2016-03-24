---
layout: post
title: Leetcode Problem 16 Summary
date: 2016-03-24
categories: blog
tags: [study]

---

#题目描述

输入一个array，有n个integer，还有一个integer型target。需要找到array中的三个数，使这三个数的加和是最接近target。

#思路

对原array排序，第一个数为从0到n－2的遍历，每次遍历分别选择i+1为第二个元素，n为第三个元素，当当前加和大于target时我们需要将第三个元素左移，小于则反之。通过这种方法将时间复杂度从O(n^3)降到O(n^2)。

#Java代码

    public class Solution {
        public int threeSumClosest(int[] nums, int target) {
            Arrays.sort(nums);
            int ret = 0;
        
            if (nums.length < 3) {
                for (int i = 0; i < nums.length; i++) {
                    ret += nums[i];
                }
                return ret;
            }
            

            ret = nums[0] + nums[1] + nums[2];
            for (int i = 0; i < nums.length - 2; i++) {
                // first value iteration
                int j = i + 1;
                int k = nums.length - 1;
                while (j < k) {
                    // second & third value iteration
                    int tmp1 = Math.abs(target - ret);
                    int tmpSum = nums[i] + nums[j] + nums[k];
                    int tmp2 = Math.abs(target - tmpSum);
                    if (tmp1 == 0) {
                        return ret;
                    }
                    if (tmp1 > tmp2) {
                        ret = tmpSum;
                    }
                    if (tmpSum > target) {
                        k--;
                    } else {
                        j++;
                    }
                }
            }
            return ret;
        }
    }