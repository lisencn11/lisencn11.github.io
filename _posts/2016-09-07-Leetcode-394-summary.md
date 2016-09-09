---
layout: post
title: Leetcode Problem 394 Summary
date: 2016-09-07
categories: blog
tags: [study]

---

# 题目

Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

* s = "3[a]2[bc]", return "aaabcbc".
* s = "3[a2[c]]", return "accaccacc".
* s = "2[abc]3[cd]ef", return "abcabccdcdcdef".

# 我的算法

DFS算法，backtrack，每次都算最外一层的情况。

# 代码

{% highlight java %}
public class Solution {
    public String decodeString(String s) {
        StringBuilder ret = new StringBuilder();
        int len = s.length();
        int pre = -1;
        int left = -1;
        int numPt = -1;
        int count = 0;
        for (int i = 0; i < len; i++) {
            if (s.charAt(i) == '[' && count == 0) {
                left = i;
                count++;
            } else if (s.charAt(i) == '[' && count > 0) {
                count++;
            }
            if (s.charAt(i) == ']' && count > 1) {
                count--;
            } else if (s.charAt(i) == ']' && count == 1) {
                String sub = decodeString(s.substring(left + 1, i));
                numPt = pre + 1;
                while (isChar(s, numPt)) numPt++;
                int times = Integer.parseInt(s.substring(numPt, left));
                ret.append(s.substring(pre + 1, numPt));
                for (int j = 0; j < times; j++) ret.append(sub);
                pre = i;
                count--;
            }
        }
        ret.append(s.substring(pre + 1, len));
        return ret.toString();
    }
    
    private boolean isChar(String s, int index) {
        char curr = s.charAt(index);
        if (curr >= '0' && curr <= '9') return false;
        if (curr == ']' || curr == '[') return false;
        return true;
    }
}
{% endhighlight %}