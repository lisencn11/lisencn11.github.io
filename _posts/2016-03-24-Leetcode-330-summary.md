---
layout: post
title: Leetcode Problem 330 Summary
date: 2016-03-24
categories: blog
tags: [study]

---

#题目描述

输入一个sorted integer positive array，向里面添加元素使得1到n中的任何数可以被array中的元素和表示。输出所需添加的元素的最小个数。

#思路

miss代表当前最小的不确定是否能表示为加和的数，即miss以前的所有数都可以被表示，那么如果我们有miss或加上miss，则miss+(miss-1)的数都可以被以加和形式表示。

#Java代码

    public class Solution {
        public int minPatches(int[] nums, int n) {
            int add = 0, i = 0;
            double miss = 1;
        
            while (miss <= n) {
                if (i < nums.length && nums[i] <= miss) {
                    miss += nums[i++];
                } else {
                    miss += miss;
                    add++;
                }
            }
            return add;
        }
    }

#注意

nums[i] <= miss判断是否可以使用已有的数组元素，如果不满足这个条件说明nums[i] > miss，如果此时用miss+nums[i]则会缺失从miss到nums[i]-1间的元素表示。