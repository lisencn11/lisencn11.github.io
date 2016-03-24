---
layout: post
title: Leetcode Problem 34 Summary
date: 2016-03-24
categories: blog
tags: [study]

---

#题目描述

输入一个有序可重复数组，和一个目标数，寻找目标数在数组中的范围，输出目标数在数组中的起始和终止位置。如果没找到则输出[-1, -1]。例如[5, 7, 7, 8, 8, 10]，目标值是8，则输出[3, 4]。

#思路

###二分查找

整个过程可拆分为两次二分查找，第一次查找起始位置，第二次查找终止位置。时间复杂度为O(logn)。

#Java代码

    public class Solution {
        public int[] searchRange(int[] nums, int target) {
            int low = 0;
            int high = nums.length - 1;
            int[] range = new int[2];
            range[0] = -1;
            range[1] = -1;
            
            // if target not in the array range, return directly
            if (target < nums[0] || target > nums[high]) {
                return range;
            }
            
            // find starting position first
            while (low <= high) {
                int mid = low + (high - low) / 2;
                if (nums[mid] > target) {
                    high = mid - 1;
                } else if (nums[mid] < target) {
                    low = mid + 1;
                } else {
                    if (mid == 0 || nums[mid - 1] != target) {
                        range[0] = mid;
                        break;
                    } else {
                        high = mid - 1;
                    }
                }
            }

            // if starting point is not found, return directly
            if (range[0] == -1) {
                return range;
            }
            
            // looking for the ending position
            low = 0;
            high = nums.length - 1;
            while (low <= high) {
                int mid = low + (high - low) / 2;
                if (nums[mid] > target) {
                    high = mid - 1;
                } else if (nums[mid] < target) {
                    low = mid + 1;
                } else {
                    if (mid == (nums.length - 1) || nums[mid + 1] != target) {
                        range[1] = mid;
                        break;
                    } else {
                        low = mid + 1;
                    }
                }
            }
        
            return range;
        }
    }