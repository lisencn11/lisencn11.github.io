---
layout: post
title: Leetcode Problem 187 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

For example,

Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",

Return:  
["AAAAACCCCC", "CCCCCAAAAA"].


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

# 二刷

问题在于没有找到办法判断是第二次还是第三次及以上。

{% highlight java %}
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        int[] shift = new int[26];
        char[] reverse = new char[4];
        char[] str = s.toCharArray();
        shift[(int) ('C' - 'A')] = 1;
        shift[(int) ('G' - 'A')] = 2;
        shift[(int) ('T' - 'A')] = 3;
        reverse[0] = 'A';
        reverse[1] = 'C';
        reverse[2] = 'G';
        reverse[3] = 'T';
        
        int p1 = 0;
        int p2 = 0;
        int encode = 0;
        Set<Integer> set = new HashSet<>();
        Set<Integer> ret = new HashSet<>();
        while (p2 < 10 && p2 < str.length) {
            char curr = str[p2];
            encode += shift[(int) (curr - 'A')] << (p2 * 2);
            p2++;
        }
        
        set.add(encode);
        while (p2 < str.length) {
            char curr = str[p2];
            encode >>= 2;
            encode += shift[(int) (curr - 'A')] << 18;
            if (!set.add(encode)) ret.add(encode);
            p2++;
        }
        
        List<String> list = new ArrayList<>();
        for (Integer i : ret) {
            list.add(itos(i, reverse));
        }
        return list;
    }
    
    private String itos(int num, char[] reverse) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 10; i++) {
            int index = (num & 3);
            num >>= 2;
            sb.append(reverse[index]);
        }
        return sb.toString();
    }
}
{% endhighlight %}