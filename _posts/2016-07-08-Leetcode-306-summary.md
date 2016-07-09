---
layout: post
title: Leetcode Problem 306 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**加法数**是指一个字符串每个字符都是数字，其中至少有三个数，每一个数都是前两个数的加和。

如："112358"可以拆分为1, 1, 2, 3, 5, 8  
"199100199"可以拆分成1, 99, 100, 199

加法数中的数不能以0开头，即不存在03这样的数

**输入**一个字符串

**输出**该字符串是否是加法数

# 我的算法

为了避免字符串过长，我们使用BigInteger存储数。算法很简单，从头到尾遍历尝试前两个数的各种组合，判断是否能符合加法数。

# 代码

{% highlight java %}
import java.math.BigInteger;

public class Solution {
    public boolean isAdditiveNumber(String num) {
        if (num == null || num.length() < 3) {
            return false;
        }
        
        for (int i = 1; i <= num.length() / 2; i++) {
            if (num.charAt(0) == '0' && i > 1) return false;
            BigInteger x1 = new BigInteger(num.substring(0, i));
            for (int j = 1; Math.max(i, j) <= num.length() - i - j; j++) {
                if (num.charAt(i) == '0' && j > 1) break;
                BigInteger x2 = new BigInteger(num.substring(i, i + j));
                if (isValid(x1, x2, i + j, num)) return true;
            }
        }
        return false;
    }
    
    private boolean isValid(BigInteger x1, BigInteger x2, int start, String num) {
        if (start == num.length()) return true;
        x2 = x2.add(x1);
        x1 = x2.subtract(x1);
        String str = x2.toString();
        return num.startsWith(str, start) && isValid(x1, x2, start + str.length(), num);
    }
}
{% endhighlight %}