---
layout: post
title: Leetcode Problem 384 Summary
date: 2016-08-20
categories: blog
tags: [study]

---

# 题目

Shuffle a set of numbers without duplicates.

Example:

// Init an array with set 1, 2, and 3.  
int[] nums = {1,2,3};  
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result.   Any permutation of [1,2,3] must equally likely to be returned.  
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].  
solution.reset();

// Returns the random shuffling of array [1,2,3].  
solution.shuffle();

# 我的算法

参考了 Java 的 Collections.shuffle() 源码，从最后一个元素开始，对于 index 为 i 的元素，产生一个从 0 到 i 的随机数，将这两个 index 的元素进行对换。

# 代码

{% highlight java %}
public class Solution {
    int[] nums;
    int[] shuffle;
    Random random;

    public Solution(int[] nums) {
        this.nums = nums;
        shuffle = new int[nums.length];
        random = new Random();
        for (int i = 0; i < nums.length; i++) shuffle[i] = nums[i];
    }
    
    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        for (int i = 0; i < nums.length; i++) shuffle[i] = nums[i];
        return shuffle;
    }
    
    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        for (int i = shuffle.length; i > 1; i--) swap(shuffle, i - 1, random.nextInt(i));
        return shuffle;
    }
    
    private void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
{% endhighlight %}