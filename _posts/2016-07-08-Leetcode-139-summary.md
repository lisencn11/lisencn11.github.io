---
layout: post
title: Leetcode Problem 139 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**输入**一个字符串s，一个字典Set<String>。

**输出**字符串是否能由字典中的String拼接而成，字典中的String可以重复使用。

例如  
>s = "leetcode",
>
>dict = ["leet", "code"]
>
# 我的算法

最开始简单的使用递归，后来提交后发现极端测例下递归时间复杂度极高。改用DP，纪录从0到当前位受否能够使用字典表示。

# 代码

{% highlight java %}
public class Solution {
    public boolean wordBreak(String s, Set<String> wordDict) {
        if (s == null || s.length() == 0) return true;
        
        boolean[] segment = new boolean[s.length() + 1];
        for (int i = 0; i < segment.length; i++) {
            segment[i] = false;
        }
        segment[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = i - 1; j >= 0; j--) {
                if (wordDict.contains(s.substring(j, i)) && segment[j]) {
                    segment[i] = true;
                    break;
                }
            }
        }
        
        return segment[s.length()];
    }
}
{% endhighlight %}