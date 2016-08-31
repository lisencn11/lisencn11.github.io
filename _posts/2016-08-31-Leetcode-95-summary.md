---
layout: post
title: Leetcode Problem 267 Summary
date: 2016-08-31
categories: blog
tags: [study]

---

# 题目

Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

For example:

Given s = "aabb", return ["abba", "baab"].

Given s = "abc", return [].

Hint:

If a palindromic permutation exists, we just need to generate the first half of the string.
To generate all distinct permutations of a (half of) string, use a similar approach from: Permutations II or Next Permutation.

# 我的算法

先判断能否构成 palindrome ，在生成即可，由于 HashMap 相对于其他数据结构速度较慢，所以自己下意识的避免使用它，但是实际面试的时候它可以有效减少代码量，以后还是应该直接用，而不是用其他数据结构进行替代。

# 代码

{% highlight java %}
public class Solution {
    public List<String> generatePalindromes(String s) {
        if (s == null || s.length() == 0) return new ArrayList<>();
        char[] str = s.toCharArray();
        int[] counts = new int[128];
        for (int i = 0; i < str.length; i++) counts[(int) str[i]]++;
        int count = 0;
        char distinct = ' ';
        for (int i = 0; i < counts.length; i++) {
            if (counts[i] % 2 == 1) {
                count++;
                distinct = (char) i;
            }
        }
        if (count > 1) return new ArrayList<>();
        
        List<Pair> cache = new ArrayList<>();
        for (int i = 0; i < counts.length; i++) {
            if (counts[i] != 0) {
                Pair pair = new Pair((char) i, counts[i]);
                cache.add(pair);
            }
        }
        
        List<String> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        int len = str.length;
        generate(list, cache, sb, len, distinct);
        return list;
    }
    
    private void generate(List<String> list, List<Pair> cache, StringBuilder sb, int len, char distinct) {
        if (len == 0) {
            list.add(sb.toString());
            return;
        }
        if (len == 1) {
            int p = sb.length() / 2;
            sb.insert(p, distinct);
            list.add(sb.toString());
            sb.deleteCharAt(p);
            return;
        }
        
        for (int i = 0; i < cache.size(); i++) {
            Pair pair = cache.get(i);
            char c = pair.c;
            int count = pair.count;
            if (count >= 2) {
                sb.insert(0, c);
                sb.append(c);
                pair.count -= 2;
                generate(list, cache, sb, len - 2, distinct);
                pair.count += 2;
                sb.deleteCharAt(sb.length() - 1);
                sb.deleteCharAt(0);
            }
        }
    }
    
    class Pair {
        char c;
        int count;
        public Pair(char c, int count) {
            this.c = c;
            this.count = count;
        }
    }
}
{% endhighlight %}