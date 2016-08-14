---
layout: post
title: Leetcode Problem 169 Summary
date: 2016-08-13
categories: blog
tags: [study]

---

# 题目

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

# 我的算法

Moore Voting Algorithm: 若一个数出现次数大于 n/2 次，我们可以假设为前 n/2 个数相同，设置计数器，遇到相同的加一，不同的减一，减到 0 换数，则最后记录的就是次数大于 n/2 次的，因为它加了 n/2 次以上。O(n)时间复杂度。

# 代码

{% highlight java %}
public class Solution {
    public int majorityElement(int[] nums) {
        int times = 1;
        int majority = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            if (times == 0) {
                times = 1;
                majority = nums[i];
            } else {
                if (majority == nums[i]) times++;
                else times--;
            }
        }
        
        return majority;
    }
}
{% endhighlight %}

# 拓展算法

1. Bit Manipulation: 用“与”操作统计二进制每一位上 1 的数量，然后选去大于 n / 2 的选择，最后得出结果，时间复杂度为 O(32n) ，即 O(n) 。
2. HashMap: key 为每个数， value 为每个数出现的次数。
3. Sorting: 排序后选择最中间的元素即可。
4. Randomization: 产生一个随机 index ，count 这个元素出现的次数是否大于 n / 2 。