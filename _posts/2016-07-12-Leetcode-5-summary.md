---
layout: post
title: Leetcode Problem 5 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

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

# 二刷

{% highlight java %}
public class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) return s;
        int start = 0;
        int end = 0;
        char[] str = s.toCharArray();
        int len = str.length;
        boolean[][] isPalindrome = new boolean[len][len];
        for (int i = 0; i < len; i++) isPalindrome[i][i] = true;
        for (int i = 0; i < len - 1; i++) {
            if (str[i] == str[i + 1]) {
                isPalindrome[i][i + 1] = true;
                start = i;
                end = i + 1;
            } else {
                isPalindrome[i][i + 1] = false;
            }
            
        }
        int countOdd = 0;
        int countEven = 0;
        for (int step = 2; step < len; step++) {
            if (step % 2 == 0) countEven = 0;
            else countOdd = 0;
            for (int i = 0; i + step < len; i++) {
                if (isPalindrome[i + 1][i + step - 1] && str[i] == str[i + step]) {
                    isPalindrome[i][i + step] = true;
                    if (step % 2 == 0) countEven++;
                    else countOdd++;
                    start = i;
                    end = i + step;
                } else {
                    isPalindrome[i][i + step] = false;
                }
            }
            if (countOdd == 0 && countEven == 0) return s.substring(start, end + 1);
        }
        return s.substring(start, end + 1);
    }
}
{% endhighlight %}

# discuss

我想复杂了：实际上对每个元素分别尝试奇数和偶数的扩展，看最长是多长。

{% highlight java %}
public class Solution {
    private int lo, maxLen;
    
    public String longestPalindrome(String s) {
    	int len = s.length();
    	if (len < 2)
    		return s;
    	
        for (int i = 0; i < len-1; i++) {
         	extendPalindrome(s, i, i);  //assume odd length, try to extend Palindrome as possible
         	extendPalindrome(s, i, i+1); //assume even length.
        }
        return s.substring(lo, lo + maxLen);
    }
    
    private void extendPalindrome(String s, int j, int k) {
    	while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
    		j--;
    		k++;
    	}
    	if (maxLen < k - j - 1) {
    		lo = j + 1;
    		maxLen = k - j - 1;
    	}
    }
}
{% endhighlight %}