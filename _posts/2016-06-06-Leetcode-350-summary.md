---
layout: post
title: Leetcode Problem 350 Summary
date: 2016-06-06
categories: blog
tags: [study]

---

# 题目

输入两个整型数组，要求返回一个包含两个数组共有元素的整型数组，相同的元素返回多次。


# 算法

与349题思路相同，不过这回相同元素返回多次，所以我们需要使用HashMap，而非HashSet，Key是数组的值，Value是对应Key在第一个数组的出现次数，遍历第二个数组时只需用List存下Key存在且Value大于0的Key即可。

# 代码

{% highlight java %}
public class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> numToFreq = new HashMap<>();
        for (int i = 0; i < nums1.length; i++) {
            if (!numToFreq.containsKey(nums1[i])) numToFreq.put(nums1[i], 0);
            numToFreq.put(nums1[i], numToFreq.get(nums1[i]) + 1);
        }
        
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < nums2.length; i++) {
            if (numToFreq.containsKey(nums2[i]) && numToFreq.get(nums2[i]) != 0) {
                numToFreq.put(nums2[i], numToFreq.get(nums2[i]) - 1);
                res.add(nums2[i]);
            }
        }
        
        int[] ret = new int[res.size()];
        int iter = 0;
        for (Integer i : res) ret[iter++] = i;
        
        return ret;
    }
}
{% endhighlight %}

