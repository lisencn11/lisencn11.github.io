---
layout: post
title: Leetcode Problem 320 Summary
date: 2016-08-21
categories: blog
tags: [study]

---

# 题目

Write a function to generate the generalized abbreviations of a word.

Example:  
Given word = "word", return the following list (order does not matter):

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

# 我的算法

使用递归方法可以解决，下面附上我的代码和discuss中高得票数的代码，相比之下我的代码太丑陋了，自惭形秽。。。

# 代码

{% highlight java %}
public class Solution {
    public List<String> generateAbbreviations(String word) {
        if (word == null || word.length() == 0) {
            List<String> list = new ArrayList<>();
            list.add(word);
            return list;
        }
        if (word.length() == 1) {
            List<String> list = new ArrayList<>();
            list.add(word);
            list.add("1");
            return list;
        }
        
        List<String> substring = generateAbbreviations(word.substring(0, word.length() -1));
        
        List<String> ret = new ArrayList<>();
        char currChar = word.charAt(word.length() - 1);
        for (int i = 0; i < substring.size(); i++) {
            String iter = substring.get(i);
            int intIndex = iter.length() - 1;
            while (intIndex >= 0 && iter.charAt(intIndex) >= '0' && iter.charAt(intIndex) <= '9') intIndex--;
            String num = (intIndex == iter.length() - 1) ? null : iter.substring(intIndex + 1, iter.length());
            int count = (num == null) ? -1 : (Integer.parseInt(num) + 1);
            String plusChar = iter + word.charAt(word.length() - 1);
            String plusNum = (count == -1) ? (iter + "1") : (iter.substring(0, intIndex + 1) + Integer.toString(count));
            ret.add(plusChar);
            ret.add(plusNum);
        }
        
        return ret;
    }
}
{% endhighlight %}

# discuss

{% highlight java %}
public List<String> generateAbbreviations(String word){
    List<String> ret = new ArrayList<String>();
    backtrack(ret, word, 0, "", 0);

    return ret;
}

private void backtrack(List<String> ret, String word, int pos, String cur, int count){
    if(pos==word.length()){
        if(count > 0) cur += count;
        ret.add(cur);
    }
    else{
        backtrack(ret, word, pos + 1, cur, count + 1);
        backtrack(ret, word, pos+1, cur + (count>0 ? count : "") + word.charAt(pos), 0);
    }
}
{% endhighlight %}