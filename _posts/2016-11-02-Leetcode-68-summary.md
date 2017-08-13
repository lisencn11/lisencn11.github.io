---
layout: post
title: Leetcode Problem 68 Summary
date: 2016-11-02
categories: blog
tags: [study]

---

# 题目

Given an array of words and a length L, format the text such that each line has exactly L characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly L characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

For example  
words: ["This", "is", "an", "example", "of", "text", "justification."]  
L: 16.

Return the formatted lines as:

[  
   "This    is    an",  
   "example  of text",  
   "justification.  "  
]

Note: Each word is guaranteed not to exceed L in length.

Corner Cases:  
A line other than the last line might contain only one word. What should you do in this case?
In this case, that line should be left-justified.

# 我的算法

计算出每行总共有多少个空格，除以words interval的数量后向上取整。

# 代码

{% highlight java %}
public class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        if (words == null || words.length == 0) return new ArrayList<>();
        
        List<String> result = new ArrayList<>();
        List<String> strsInLine = new ArrayList<>(); 
        int charCount = 0;
        
        for (String word : words) {
            if (charCount + strsInLine.size() + word.length() > maxWidth) {
                if (strsInLine.size() == 1) {
                    result.add(getLeftJustified(strsInLine, maxWidth, charCount));
                } else {
                    result.add(getDualJustified(strsInLine, maxWidth, charCount));
                }
                strsInLine = new ArrayList<>();
                charCount = 0;
            }
            
            strsInLine.add(word);
            charCount += word.length();
        }
        
        if (!strsInLine.isEmpty()) {
            result.add(getLeftJustified(strsInLine, maxWidth, charCount));
        }
        
        return result;
    }
    
    private String getDualJustified(List<String> strsInLine, int maxWidth, int charCount) {
        StringBuilder lineBuilder = new StringBuilder();
        int spaces = maxWidth - charCount;
        int interval = strsInLine.size() - 1;
        for (String str : strsInLine) {
            lineBuilder.append(str);
            if (interval > 0) {
                int currSpace = (int) Math.ceil((double) spaces / interval);
                spaces -= currSpace;
                interval--;
                while (currSpace-- > 0) {
                    lineBuilder.append(" ");
                }
            }
        }
        return lineBuilder.toString();
    }
    
    private String getLeftJustified(List<String> strsInLine, int maxWidth, int charCount) {
        StringBuilder lineBuilder = new StringBuilder();
        for (String str : strsInLine) {
            lineBuilder.append(str);
            if (lineBuilder.length() < maxWidth) {
                lineBuilder.append(" ");
            }
        }
        while (lineBuilder.length() < maxWidth) {
            lineBuilder.append(" ");
        }
        return lineBuilder.toString();
    }
}
{% endhighlight %}