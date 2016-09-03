---
layout: post
title: Leetcode Problem 131 Summary
date: 2016-07-06
categories: blog
tags: [study]

---

# 题目

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",  
Return

[  
  ["aa","b"],  
  ["a","a","b"]  
]

# 我的算法

使用DFS算法，回溯实现。

设计函数**partition(String s, int start, List<List<String>> result, List<String> list)**，这个函数中**s**为原String，**start**为当前位置，**result**为最后结果，**list**为从位置0到start-1已经划分好的List<String>。

我们需要在这个函数中首先判断是否到达结尾，如果到达则添加划分，如果没到达，那么我们从start开始，一位一位组成String并判断是否是Palindrome，如果是则DFS，否则什么也不做。

# 代码

{% highlight java %}
public class Solution {
    private boolean isPanlindrome(String s) {
        int low = 0;
        int high = s.length() - 1;
        while (low <= high && s.charAt(low) == s.charAt(high)) {
            low++;
            high--;
        }
        if (low <= high) {
            return false;
        } else {
            return true;
        }
    }
    
    private void partition(char[] str, int start, List<List<String>> result, List<String> list) {
        if (start == str.length) {
            result.add(new ArrayList<String>(list));
            return;
        }
        
        StringBuilder string = new StringBuilder();
        for (int i = start; i < str.length; i++) {
            string.append(str[i]);
            if (isPanlindrome(string.toString())) {
                list.add(string.toString());
                partition(str, start + string.length(), result, list);
                list.remove(list.size() - 1);
            }
        }
    }
    
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<List<String>>();
        char[] str = s.toCharArray();
        partition(str, 0, result, new ArrayList<String>());
        return result;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<List<String>> partition(String s) {
        int len = s.length();
        List<List<String>> ret = new ArrayList<>();
        List<String> list = new ArrayList<>();
        partitionHelper(s, len, ret, list);
        return ret;
    }
    
    private void partitionHelper(String s, int index, List<List<String>> ret, List<String> list) {
        if (index == 0) {
            ret.add(new ArrayList<>(list));
            return;
        }
        
        for (int i = index - 1; i >=0; i--) {
            if (isValid(s.substring(i, index))) {
                list.add(0, s.substring(i, index));
                partitionHelper(s, i, ret, list);
                list.remove(0);
            }
        }
    }
    
    private boolean isValid(String s) {
        int p1 = 0;
        int p2 = s.length() - 1;
        while (p1 < p2) {
            if (s.charAt(p1++) != s.charAt(p2--)) return false;
        }
        return true;
    }
}
{% endhighlight %}