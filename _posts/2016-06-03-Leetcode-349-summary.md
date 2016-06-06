---
layout: post
title: Leetcode Problem 349 Summary
date: 2016-06-03
categories: blog
tags: [study]

---

# 题目

输入两个整型数组，要求返回一个包含两个数组共有元素的整型数组，相同的元素只返回一次。


# 算法

使用两个HashSet，第一个Set，遍历并存储第一个数组中的所有元素；第二个Set，遍历第二个数组，将第一个Set中存在的元素放入第二个Set。第二个Set即为共有元素，算法复杂度为O(n)。

# 代码

```java

public class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1 == null || nums2 == null) {
            return null;
        }
        
        Set compare = new HashSet<Integer>();
        Set result = new HashSet<Integer>();
        
        for (int i = 0; i < nums1.length; i++) {
            compare.add(new Integer(nums1[i]));
        }
        
        for (int i = 0; i < nums2.length; i++) {
            if (compare.contains(new Integer(nums2[i]))) {
                result.add(new Integer(nums2[i]));
            }
        }
        
        Iterator<Integer> iter = result.iterator();
        int[] ret = new int[result.size()];
        
        for (int i = 0; iter.hasNext(); i++) {
            ret[i] = iter.next().intValue();
        }
        
        return ret;
    }
}

```

