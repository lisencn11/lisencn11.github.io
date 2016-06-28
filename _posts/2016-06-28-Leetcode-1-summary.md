---
layout: post
title: Leetcode Problem 1 Summary
date: 2016-06-28
categories: blog
tags: [study]

---

# 题目

**输入**一个非负整数n。

**输出**从0 <= x < 10<sup>n</sup>之间所有满足此条件的数的个数：这个数的每一位都不相同。例：1234567

输入：2;

输出：2 排除[11, 22, 33, 44, 55...]

# 我的算法

动态规划思路：当n为0，返回1。当n为1，返回9 + 1。此时发现存在递推关系：假设我们已知n=i-1时，即i-1位数组成的每位唯一数，求n=i我们只需要对这些数最后一位加一个数即可，最后一位有(10 - (i - 1))种可能，故为a<sub>n</sub> = a<sub>n-1</sub> * (10 - (i - 1))。最后将a<sub>n</sub>加和即可。

# 代码

{% highlight java %}
public class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) {
            return 1;
        }
        int[] totalCount = new int[10];
        int[] nthCount = new int[10];
        totalCount[0] = 10;
        nthCount[0] = 9;
        
        if (1 == n) {
            return totalCount[0];
        }
        
        for (int i = 1, j = 9; i < 10; i++, j--) {
            nthCount[i] = nthCount[i - 1] * j;
            totalCount[i] = totalCount[i - 1] + nthCount[i];
            if ((i + 1) == n) {
                return totalCount[i];
            }
        }
        
        if (n > 10) {
            return totalCount[9];
        }
        return -1;
    }
}
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) {
            return new ArrayList<List<String>>();
        }
        
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        List<String> list = null;
        List<List<String>> result = new ArrayList<List<String>>();
        int i = 0;
        i++;
        
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
{% endhighlight %}