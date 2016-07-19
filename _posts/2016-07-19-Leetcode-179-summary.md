---
layout: post
title: Leetcode Problem 179 Summary
date: 2016-07-19
categories: blog
tags: [study]

---

# 题目

**输入**一个非负整型数组

**输出**数组内元素所能拼接成的最大的数

例如：

>given [3, 30, 34, 5, 9], the largest formed number is 9534330.

# 我的算法

观察发现，越靠前的位越大，拼接时越应放在前面，但是对于长度不等的两个整型如 333 和333453 该如何判断？

第一想法是先判断两个数的公共长度部分，若不等则返回结果，若想等则继续判断下一位和短的整型的首位大小，直到比出大小为止。但是存在漏洞如，比较起来相当麻烦。

结合discuss中的看到简单的对两个String拼接后直接比较结果即可。

# 代码

{% highlight java %}
public class Solution {
    Comparator<String> comp = new Comparator<String>() {
        public int compare(String o1, String o2) {
            String str1 = o1 + o2;
            String str2 = o2 + o1;
            return str2.compareTo(str1);
        }
    };

    public String largestNumber(int[] nums) {
        if (nums == null || nums.length == 0) return new String();
        String[] sbs = new String[nums.length];
        Queue<String> queue = new PriorityQueue<String>(comp);
        StringBuilder ret = new StringBuilder();
        
        for (int i = 0; i < nums.length; i++) {
            sbs[i] = Integer.toString(nums[i]);
            queue.offer(sbs[i]);
        }
        
        if (queue.peek().equals("0")) return "0";
        
        while (!queue.isEmpty()) ret.append(queue.poll());
        
        return ret.toString();
    }
}
{% endhighlight %}