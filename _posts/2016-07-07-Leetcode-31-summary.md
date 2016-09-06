---
layout: post
title: Leetcode Problem 31 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

1,2,3 → 1,3,2  
3,2,1 → 1,2,3  
1,1,5 → 1,5,1

# 我的算法

观察发现，总是尾部一部分子数组达到完全逆序，然后交换子数组中某一个元素和子数组的前一个元素，然后排序。

1. 找到完全逆序的前一位a<sub>i-1</sub>
2. 找到完全逆序中所有大于a<sub>i-1</sub>中最小的那一位
3. 交换这两个数，然后对逆序序列重新排序即可。

# 代码

{% highlight java %}
public class Solution {
    public void nextPermutation(int[] nums) {
        if (nums == null || nums.length < 2) {
            return;
        }
        
        int startPoint = -1;
        int exchangePoint = 0;
        int exchange;
        int low;
        int high;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] < nums[i + 1]) {
                startPoint = i;
            }
        }
        
        if (startPoint == -1) {
            low = 0;
            high = nums.length - 1;
            while (low < high) {
                exchange = nums[low];
                nums[low] = nums[high];
                nums[high] = exchange;
                low++;
                high--;
            }
            return;
        }
        
        for (int i = nums.length - 1; i > startPoint; i--) {
            if (nums[startPoint] < nums[i]) {
                exchangePoint = i;
                break;
            }
        }
        
        exchange = nums[startPoint];
        nums[startPoint] = nums[exchangePoint];
        nums[exchangePoint] = exchange;
        
        low = startPoint + 1;
        high = nums.length - 1;
        while (low < high) {
            exchange = nums[low];
            nums[low] = nums[high];
            nums[high] = exchange;
            low++;
            high--;
        }
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public void nextPermutation(int[] nums) {
        int p1 = nums.length - 1;
        while (p1 > 0 && nums[p1 - 1] >= nums[p1]) p1--;
        if (p1 == 0) {
            Arrays.sort(nums);
            return;
        }
        int swap = p1 - 1;
        p1 = nums.length - 1;
        while (p1 > swap && nums[p1] <= nums[swap]) p1--; 
        swap(nums, swap, p1);
        p1 = swap + 1;
        int p2 = nums.length - 1;
        while (p1 < p2) swap(nums, p1++, p2--);
    }
    
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
{% endhighlight %}