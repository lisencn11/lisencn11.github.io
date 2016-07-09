---
layout: post
title: Leetcode Problem 187 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**输入**一段DNA序列，其中包含'A', 'C', 'G', 'T'四种字符。

**输出**找到所有在这段DNA中重复的长度为10的子序列

例如

s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

返回["AAAAACCCCC", "CCCCCAAAAA"]，表示这两段长度为10的子序列有重复。


# 我的算法

使用哈希表HashSet对每个长度为10的子序列进行存储，若发现某个子序列已经存在在HashSet中，则表示其为重复序列。

# 代码

{% highlight java %}
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<String> firstTime = new HashSet<String>();
        Set<String> secondTime = new HashSet<String>();
        List<String> ret = new ArrayList<String>();
        
        for (int i = 0; i < s.length() - 9; i++) {
            String iter = s.substring(i, i + 10);
            if (!firstTime.contains(iter)) {
                firstTime.add(iter);
            } else {
                if (!secondTime.contains(iter)) {
                    secondTime.add(iter);
                    ret.add(iter);
                }
            }
        }
        return ret;
    }
}
{% endhighlight %}

# 改进

因为只有四种可能的字符，所以可以通过两位二进制表示四种字符，然后将子字符串转化为Integer，代码速度有提升效果。

# 改进后代码

{% highlight java %}
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<Integer> firstTime = new HashSet<Integer>();
        Set<Integer> secondTime = new HashSet<Integer>();
        List<String> ret = new ArrayList<String>();
        
        char[] map = new char[26];
        map['C' - 'A'] = 1;
        map['G' - 'A'] = 2;
        map['T' - 'A'] = 3;
        char[] str = s.toCharArray();
        
        for (int i = 0; i < str.length - 9; i++) {
            int v = 0;
            for (int j = i; j < i + 10; j++) {
                v <<= 2;
                v |= map[str[j] - 'A'];
            }
            if (!firstTime.contains(v)) {
                firstTime.add(v);
            } else {
                if (!secondTime.contains(v)) {
                    secondTime.add(v);
                    ret.add(s.substring(i, i + 10));
                }
            }
        }
        return ret;
    }
}
{% endhighlight %}