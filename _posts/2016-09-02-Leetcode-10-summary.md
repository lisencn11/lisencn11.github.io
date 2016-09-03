---
layout: post
title: Leetcode Problem 10 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.  
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:  
isMatch("aa","a") → false  
isMatch("aa","aa") → true  
isMatch("aaa","aa") → false  
isMatch("aa", "a*") → true  
isMatch("aa", ".*") → true  
isMatch("ab", ".*") → true  
isMatch("aab", "c*a*b") → true

# 我的算法

backtrack 实现

# 代码

{% highlight java %}
public class Solution {
    public boolean isMatch(String s, String p) {
        if (s.length() == 0 && p.length() == 0) return true;
        if (p.length() == 0) return false;
        if (s.length() == 0) {
            for (int i = p.length() - 1; i >= 0; i -= 2) {
                if (p.charAt(i) != '*') return false;
            }
        }
        
        int m = s.length(), n = p.length();
        if (p.charAt(n - 1) == '.') {
            return isMatch(s.substring(0, m - 1), p.substring(0, n - 1));
        } else if (p.charAt(n - 1) == '*') {
            char regex = p.charAt(n - 2);
            for (int i = m; i >= 0; i--) {
                if (i != m && s.charAt(i) != regex && regex != '.') {
                    break;
                }
                if (isMatch(s.substring(0, i), p.substring(0, n - 2))) {
                    return true;
                }
            }
        } else {
            if (s.charAt(m - 1) == p.charAt(n - 1)) {
                return isMatch(s.substring(0, m - 1), p.substring(0, n - 1));
            }
        }
        return false;
    }
}
{% endhighlight %}

# discuss

DP

{% highlight cpp %}
class Solution {
public:
    bool isMatch(string s, string p) {
        /**
         * f[i][j]: if s[0..i-1] matches p[0..j-1]
         * if p[j - 1] != '*'
         *      f[i][j] = f[i - 1][j - 1] && s[i - 1] == p[j - 1]
         * if p[j - 1] == '*', denote p[j - 2] with x
         *      f[i][j] is true iff any of the following is true
         *      1) "x*" repeats 0 time and matches empty: f[i][j - 2]
         *      2) "x*" repeats >= 1 times and matches "x*x": s[i - 1] == x && f[i - 1][j]
         * '.' matches any single character
         */
        int m = s.size(), n = p.size();
        vector<vector<bool>> f(m + 1, vector<bool>(n + 1, false));
        
        f[0][0] = true;
        for (int i = 1; i <= m; i++)
            f[i][0] = false;
        // p[0.., j - 3, j - 2, j - 1] matches empty iff p[j - 1] is '*' and p[0..j - 3] matches empty
        for (int j = 1; j <= n; j++)
            f[0][j] = j > 1 && '*' == p[j - 1] && f[0][j - 2];
        
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
                if (p[j - 1] != '*')
                    f[i][j] = f[i - 1][j - 1] && (s[i - 1] == p[j - 1] || '.' == p[j - 1]);
                else
                    // p[0] cannot be '*' so no need to check "j > 1" here
                    f[i][j] = f[i][j - 2] || (s[i - 1] == p[j - 2] || '.' == p[j - 2]) && f[i - 1][j];
        
        return f[m][n];
    }
};
{% endhighlight %}