---
layout: post
title: Leetcode Problem 55 Summary
date: 2016-06-08
categories: blog
tags: [study]

---

# 题目

输入一个非负整型数组，假设你一开始在index为0的地方，数组中的每个element代表能前进的最大步数，设计算法求是否能到达最后一个index。

# 算法

动态规划思路：数组从后向前扫描，纪录当前能够到达结尾的最靠前的index，如果最后这个index为0，则表示我们可以从头走到尾。

贪心算法：从前向后扫描，纪录当前能够到达的最靠后的index，如果这个index能够为**nums.length-1**，则表示我们可以从头走到尾。

# 代码

{% highlight java %}
public class Solution {
    public boolean canJump(int[] nums) {
        if (nums == null || nums.length < 1) {
            return false;
        }
        
        int lastIndexReached = nums.length - 1;
        for (int i = nums.length - 2; i >= 0; i--) {
            if (nums[i] >= (lastIndexReached - i)) {
                lastIndexReached = i;
            }
        }
        
        return lastIndexReached == 0;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public boolean canJump(int[] nums) {
        int curr = 0;
        int max = 0;
        while (curr <= max && curr < nums.length) {
            int jumpTo = nums[curr] + curr;
            max = jumpTo > max ? jumpTo : max;
            curr++;
        }
        return max >= nums.length - 1;
    }
}
{% endhighlight %}