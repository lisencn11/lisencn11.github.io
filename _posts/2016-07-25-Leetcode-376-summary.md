---
layout: post
title: Leetcode Problem 376 Summary
date: 2016-07-25
categories: blog
tags: [study]

---

# 题目

1 7 2 6 3 5 4 这种前一个数后一个数的差交替为负的序列称为 wiggle sequence。

**输入**一个整型数组

**输出**子序列 subsequence 中的最大 wiggle sequence长度

如：

Input: [1,7,4,9,2,5]  
Output: 6  
The entire sequence is a wiggle sequence.

Input: [1,17,5,10,13,15,10,5,16,8]  
Output: 7  
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

Input: [1,2,3,4,5,6,7,8,9]  
Output: 2

# 我的算法

我使用的贪心算法，比起 discuss 中的略为繁杂，需要纪录当前应该向上还是向下两个状态，当前长度，还有前一个数的大小。如果该向上却向下，则只更新节点值而不更新状态和长度，反之同理。

# 代码

{% highlight java %}
public class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        
        int status = 0;
        int pre = nums[0];
        int len = 1;
        
        for (int i = 1; i < nums.length; i++) {
            if (status == 1) {
                if (nums[i] > pre) {
                    status = -1;
                    len++;
                }
            } else if (status == -1) {
                if (nums[i] < pre) {
                    status = 1;
                    len++;
                }
            } else {
                if (nums[i] > pre) {
                    status = -1;
                    len++;
                } else if (nums[i] < pre) {
                    status = 1;
                    len++;
                }
            }
            pre = nums[i];
        }
        return len;
    }
}
{% endhighlight %}