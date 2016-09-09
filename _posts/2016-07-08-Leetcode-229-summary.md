---
layout: post
title: Leetcode Problem 229 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times. The algorithm should run in linear time and in O(1) space.

# 我的算法

多数投票算法，两个int型num变量纪录当前候选者，两个counter纪录当前对应元素次数，从前向后扫描一遍，如果等于两个候选者之一，则对应counter加一；如果均不等于，则看是否有counter已为0，如果有则替换候选者并置counter为1；如果也没有，则两个counter同时减1。

证明：  
如果存在次数大于n / 3的元素，那么它的counter至少加（n / 3） + 1次，而两个counter同时减1的操作则不到(n / 3)次，因为另一个counter不能一直减，至少要加1次再减1次，所以如果存在次数大于n / 3的元素，那么最后它一定会是两个候选者之一，接下来我们再遍历一次数组对两个候选者进行计数即可知道是否存在次数大于n / 3的元素。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> ret = new ArrayList<Integer>();
        if (nums == null || nums.length < 1) {
            return ret;
        }
        
        int num1 = nums[0];
        int num2 = 0;
        int count1 = 1;
        int count2 = 0;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            } else if (count1 == 0) {
                num1 = nums[i];
                count1 = 1;
            } else if (count2 == 0) {
                num2 = nums[i];
                count2 = 1;
            } else {
                count1--;
                count2--;
            }
        }
        count1 = 0;
        count2 = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            }
        }
        if (count1 > (nums.length / 3)) {
            ret.add(num1);
        }
        if (count2 > (nums.length / 3)) {
            ret.add(num2);
        }
        return ret;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<Integer> majorityElement(int[] nums) {
        if (nums == null || nums.length == 0) return new ArrayList<>();
        List<Integer> result = new ArrayList<>();
        int majorityA = 0;
        int countA = 0;
        int majorityB = 0;
        int countB = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == majorityA) {
                countA++;
            } else if (nums[i] == majorityB) {
                countB++;
            } else {
                if (countA == 0) {
                    countA = 1;
                    majorityA = nums[i];
                } else if (countB == 0) {
                    countB = 1;
                    majorityB = nums[i];
                } else {
                    countA--;
                    countB--;
                }
            }
        }
        countA = 0;
        countB = 0;
        for (int i = 0; i < nums.length; i++) {
            countA = nums[i] == majorityA ? countA + 1 : countA;
            countB = nums[i] == majorityB ? countB + 1 : countB;
        }
        if (countA > nums.length / 3) result.add(majorityA);
        if (countB > nums.length / 3 && majorityA != majorityB) result.add(majorityB);
        return result;
    }
}
{% endhighlight %}