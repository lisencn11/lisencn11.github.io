---
layout: post
title: Leetcode Problem 288 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

![problem288](https://lisencn11.github.io/img/problem288.png)
     
Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

Example:   
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> false  
isUnique("cart") -> true  
isUnique("cane") -> false  
isUnique("make") -> true

# 我的算法

考察HashMap以外主要考察是否考虑周全，如最开始我才用HashMap<String, Integer>以缩写作为key，重复数量作为value，若value为1表示无重复，但是isUnique()调用的参数和dictionary中又未必一样。

最终才用HashMap<String, String>，key不变，但是String的话是原dictionary中的String，若遇到重复则置为 ""，这样就能够进行判断了。

# 代码

{% highlight java %}
public class ValidWordAbbr {
    Map<String, String> map;

    public ValidWordAbbr(String[] dictionary) {
        map = new HashMap<>();
        for (int i = 0; i < dictionary.length; i++) {
            String temp = convert(dictionary[i]);
            if (!map.containsKey(temp)) {
                map.put(temp, dictionary[i]);
            } else {
                String dict = map.get(temp);
                if (!dict.equals(dictionary[i])) {
                    map.put(temp, "");
                }
            }
        }
    }

    public boolean isUnique(String word) {
        String temp = convert(word);
        if (!map.containsKey(temp)) return true;
        String dict = map.get(temp);

        return dict.equals(word);
    }
    
    private String convert(String word) {
        if (word.length() <= 2) return word;
        int len = word.length() - 2;
        return word.charAt(0) + Integer.toString(len) + word.charAt(word.length() -1);
    }
}


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa = new ValidWordAbbr(dictionary);
// vwa.isUnique("Word");
// vwa.isUnique("anotherWord");
{% endhighlight %}