---
layout: post
title: Leetcode Problem 165 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Compare two version numbers version1 and version2.

If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.  
The . character does not represent a decimal point and is used to separate number sequences.  
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:

0.1 < 1.1 < 1.2 < 13.37

# 我的算法

思想很简单，但是我在两个地方犯错了

1. String 的 split 方法在用于 "." 和 "|" 的时候要转义 String.split("\\.")
2. 1.0 和 1 是一个版本，我默认为不同

补充说明：String.split("and|or")这种用法可以同时用两个regular expression进行匹配

# 代码

{% highlight java %}
public class Solution {
    public int compareVersion(String version1, String version2) {
        String[] v1 = version1.split("\\.");
        String[] v2 = version2.split("\\.");

        for (int i = 0; i < v1.length || i < v2.length; i++) {
            int ver1 = i < v1.length ? Integer.parseInt(v1[i]) : 0;
            int ver2 = i < v2.length ? Integer.parseInt(v2[i]) : 0;
            if (ver1 < ver2) return -1;
            else if (ver1 > ver2) return 1;
        }
        
        return 0;
    }
}
{% endhighlight %}