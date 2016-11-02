---
layout: post
title: Leetcode Problem 411 Summary
date: 2016-10-17
categories: blog
tags: [study]

---

# 题目

A string such as "word" contains the following abbreviations:

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the smallest possible length such that it does not conflict with abbreviations of the strings in the dictionary.

Each number or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.

Note:

* In the case of multiple answers as shown in the second example below, you may return any one of them.
* Assume length of target string = m, and dictionary size = n. You may assume that m ≤ 21, n ≤ 1000, and log2(n) + m ≤ 20.
 
Examples: 

"apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")

"apple", ["plain", "amber", "blade"] -> "1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").

# 我的算法

暴力算法，从短到长生成abbreviation的List，对每一个abbreviation判断在dict中是否存在匹配。

# 代码

{% highlight java %}
public class Solution {
    private final static String NUMBER = "0123456789";
    Map[] maps;

    public String minAbbreviation(String target, String[] dictionary) {
        int len = target.length();
        maps = new HashMap[len + 1];
        for (int i = 0; i < len + 1; i++) {
            maps[i] = new HashMap<>();
        }
        
        for (int i = 1; i <= len; i++) {
            List<StringBuilder> abbrList = getAbbr(target, i);
            for (StringBuilder sb : abbrList) {
                boolean canPattern = false;
                for (String str : dictionary) {
                    if (checkConflict(str, sb.toString())) {
                        canPattern = true;
                        break;
                    }
                }
                if (!canPattern) {
                    return sb.toString();
                }
            }
        }
        
        return "";
    }
    
    private List<StringBuilder> getAbbr(String target, int len) {        
        Set<StringBuilder> result = new HashSet<>();
        int targetLen = target.length();
        String sub = target.substring(0, targetLen - 1);
        String curr = target.substring(targetLen - 1);
        
        if (len == 1) {
            StringBuilder abbr = new StringBuilder(String.valueOf(target.length()));
            result.add(abbr);
            if (targetLen == 1) {
                result.add(new StringBuilder(curr));
            }
            return new ArrayList<>(result);
        }
        
        if (len > targetLen) {
            return new ArrayList<>();
        }
        
        List<StringBuilder> pickList1 = getAbbr(sub, len - 1);
        List<StringBuilder> pickList2 = getAbbr(sub, len);

        
        for (StringBuilder sb : pickList1) {
            StringBuilder sb1 = new StringBuilder(sb);
            sb1.append(curr);
            result.add(sb1);
            if (!NUMBER.contains(sb.substring(sb.length() - 1, sb.length()))) {
                StringBuilder sb2 = new StringBuilder(sb);
                sb2.append("1");
                result.add(sb2);
            }
        }
        
        for (StringBuilder sb : pickList2) {
            StringBuilder sb1 = new StringBuilder(sb);
            int index = sb1.length() - 1;
            if (!NUMBER.contains(sb.substring(sb.length() - 1, sb.length()))) {
                continue;
            }
            while (index >= 0 && NUMBER.contains(sb.substring(index, index + 1))) {
                index--;
            }
            int number = Integer.parseInt(sb.substring(index + 1, sb.length())) + 1;
            sb1.delete(index + 1, sb.length());
            sb1.append(number);
            result.add(sb1);
        }
        
        return new ArrayList<>(result);
    }
    
    private boolean checkConflict(String string, String pattern) {
        int strIndex = 0, patIndex = 0;
        
        for (strIndex = 0, patIndex = 0; strIndex < string.length() || patIndex < pattern.length(); strIndex++, patIndex++) {
            if (strIndex >= string.length() || patIndex >= pattern.length()) return false;
            
            if (NUMBER.contains(pattern.substring(patIndex, patIndex + 1))) {
                int end = patIndex + 1;
                while (end < pattern.length() && NUMBER.contains(pattern.substring(end, end + 1))) {
                    end++;
                }
                Integer number = Integer.parseInt(pattern.substring(patIndex, end));
                strIndex += number - 1;
                patIndex = end - 1;
            } else {
                if (string.charAt(strIndex) != pattern.charAt(patIndex)) {
                    return false;
                }
            }
        }
        return strIndex == string.length() && patIndex == pattern.length();
    }
}
{% endhighlight %}