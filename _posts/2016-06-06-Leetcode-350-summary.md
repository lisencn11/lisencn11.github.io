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

```java

public class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1 == null || nums2 == null) {
            return null;
        }
        
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        List<Integer> list = new ArrayList<Integer>();
        
        for (int i = 0; i < nums1.length; i++) {
            Integer key = new Integer(nums1[i]);
            if (map.containsKey(key)) {
                Integer value = map.get(key);
                value++;
                map.put(key, value);
            } else {
                map.put(key, new Integer(1));
            }
        }
        
        for (int i = 0; i < nums2.length; i++) {
            Integer key = new Integer(nums2[i]);
            if (map.containsKey(key) && map.get(key) > 0) {
                Integer value = map.get(key);
                value--;
                map.put(key,value);
                list.add(key);
            }
        }
        
        int[] result = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            result[i] = list.get(i);
        }
        
        return result;
    }
}

```

