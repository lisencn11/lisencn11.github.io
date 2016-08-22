---
layout: post
title: Leetcode Problem 318 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

Example 1:  
Given ["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]  
Return 16  
The two words can be "abcw", "xtfn".

Example 2:  
Given ["a", "ab", "abc", "d", "cd", "bcd", "abcd"]  
Return 4  
The two words can be "ab", "cd".

Example 3:  
Given ["a", "aa", "aaa", "aaaa"]  
Return 0  
No such pair of words.

# 我的算法

字符串比较可以使用位操作，因为限定只有小写字母，所以可以用26位表示是否有某个字母存在，然后用与操作快速判断是否有重复字母。

# 代码

{% highlight java %}
public class Solution {
    public int maxProduct(String[] words) {
        List<Pair> list = new ArrayList<>();
        int max = 0;
        for (int i = 0; i < words.length; i++) {
            int key = translate(words[i]);
            int value = words[i].length();
            list.add(new Pair(key, value));
            for (int j = i - 1; j >= 0; j--) {
                Pair pair = list.get(j);
                if ((pair.key & key) == 0) {
                    int product = pair.value * value;
                    max = product > max ? product : max;
                }
            }
        }
        
        return max;
    }
    
    private int translate(String s) {
        char[] c = s.toCharArray();
        int num = 0;
        for (int i = 0; i < c.length; i++) {
            num |= 1 << (int)(c[i] - 'a');
        }
        
        return num;
    }
    
    class Pair {
        int key;
        int value;
        public Pair(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
}{% endhighlight %}