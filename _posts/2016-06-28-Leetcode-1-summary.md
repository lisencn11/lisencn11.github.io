---
layout: post
title: Leetcode Problem 1 Summary
date: 2016-06-28
categories: blog
tags: [study]

---

# 题目

**输入**一个由String数组。

**输出**内含List的List，将由相同数量个字符组成的String放到一个List中

输入：["eat", "tea", "tan", "ate", "nat", "bat"];

输出：[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]

# 我的算法

### 解题难点

对于这道题，有两个关键点：

1. 如何快速判断String所属List
2. 如何快速定位List

### 解决方法：

1. 通过Arrays.sort()将String排序，如果是符合条件的List应该会有相同的String
2. 使用HashMap，以排序后的String为key，以List为value，可实现O(1)的定位时间复杂度

### 改良过程：

最初的思路是通过一个权值计算，用每个字符的数量乘以ascii码，计算出一个值作为key，这样的话计算key值的复杂度是O(n)而非排序的O(nlogn)。但是在最坏的情况下不同的String构成也会计算出相同的值，所以最后还是使用排序后的String作为key。

# 代码

```java
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return new ArrayList<List<String>>();
        }
        
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        List<String> list = null;
        List<List<String>> result = new ArrayList<List<String>>();
        
        for (String s : strs) {
            char[] c = s.toCharArray();
            Arrays.sort(c);
            String stringKey = s.valueOf(c);
            if (!map.containsKey(stringKey)) {
                list = new ArrayList<String>();
                list.add(s);
                map.put(stringKey, list);
            } else {
                list = map.get(stringKey);
                list.add(s);
                map.put(stringKey, list);
            }
        }
        
        Set<String> set = map.keySet();
        for (String s : set) {
            list = map.get(s);
            result.add(list);
        }
        return result;
    }
}
```
