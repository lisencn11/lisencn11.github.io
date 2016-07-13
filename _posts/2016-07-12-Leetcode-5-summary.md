---
layout: post
title: Leetcode Problem 5 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

**输入**一个字符串String。

**输出**满足条件palindrome的最长字串。

# 我的算法

动态规划：boolean[][] isPalindrome，isPalindrome[i][j]表示从 i 到 j 的字串是否是palindrome。

基础条件是所有的isPalindrome[i][i]都为true。

isPalindrome[i][i + 1]判断两者是否相同后赋值。  
 
isPalindrome[i][j] = isPalindrome[i + 1][j - 1] && String.charAt(i) == String.charAt(j)。

# 代码

{% highlight java %}
public class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 2) {
            return s;
        }
        char[] c = s.toCharArray();
        boolean[][] isPalindrome = new boolean[c.length][c.length];
        int start = 0;
        int end = 0;
        int counter = 0;
        int preCounter = 0;
        
        for (int i = 0; i < isPalindrome.length; i++) {
            isPalindrome[i][i] = true;
            start = i;
            end = i;
        }
        for (int i = 0; i < isPalindrome.length - 1; i++) {
            if (c[i] == c[i + 1]) {
                isPalindrome[i][i + 1] = true;
                start = i;
                end = i + 1;
            } else {
                isPalindrome[i][i + 1] = false;
            }
        }
        
        for (int i = 2; i < isPalindrome.length; i++) {
            counter = 0;
            for (int j = 0; j < isPalindrome.length -i; j++) {
                int index = j + i;
                if (isPalindrome[j + 1][index - 1] && c[j] == c[index]) {
                    isPalindrome[j][index] = true;
                    counter++;
                    start = j;
                    end = index;
                } else {
                    isPalindrome[j][index] = false;
                }
            }
            if (counter == 0 && preCounter == 0) {
                break;
            }
            preCounter = counter;
        }
        
        return s.substring(start, end + 1);
    }
}
{% endhighlight %}