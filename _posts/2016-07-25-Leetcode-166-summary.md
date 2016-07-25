---
layout: post
title: Leetcode Problem 166 Summary
date: 2016-07-25
categories: blog
tags: [study]

---

# 题目

**输入**两个整型，一个除数，一个被除数。

**输出**除法结果。

除法结果有可能是整数，有可能是有限小数，也有可能是无限循环小数，但不可能是无限不循环小数。

For example,

Given numerator = 1, denominator = 2, return "0.5".  
Given numerator = 2, denominator = 1, return "2".  
Given numerator = 2, denominator = 3, return "0.(6)".  

Hint:

No scary math, just apply elementary math knowledge.   Still remember how to perform a long division?  
Try a long division on 4/9, the repeating part is obvious. Now try 4/333. Do you see a pattern?  
Be wary of edge cases! List out as many test cases as you can think of and test your code thoroughly.

# 我的算法

主要考察边界条件：

1. 除数与被除数异号
2. 被除数为 0
3. 除数为 0
4. 能够整除
5. 不能整除有循环小数
6. 循环小数从第 1 位开始或者从某一位开始
7. 循环小数中包含重复数字

第一次主要没有考虑到第 6、7 点，解决办法是用哈希表纪录， HashMap\<Point, Integer>() ， Point 中存着每一次除法的商和余数， Integer 为对应的第几位，以便插入左括号。

# 代码

{% highlight java %}
import java.awt.Point;

public class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if (denominator == 0) return "";
        if (numerator == 0) return "0";
        
        StringBuilder sb = new StringBuilder();
        Map<Point, Integer> dup = new HashMap<>();
        long posNumerator = Math.abs((long)numerator);
        long posDenominator = Math.abs((long)denominator);
        String part1 = Long.toString(posNumerator / posDenominator);
        long remain = posNumerator % posDenominator;
        remain *= 10;
        long fraction = remain / posDenominator;

        Point p = new Point((int)remain, (int)fraction);
        int index = 0;
        while (!(remain == 0 && fraction == 0) && !dup.containsKey(p)) {
            dup.put(p, index++);
            sb.append(fraction);
            remain = remain % posDenominator;
            remain *= 10;
            fraction = remain / posDenominator;
            p = new Point((int)remain, (int)fraction);
        }
        
        if (remain != 0) {
            index = dup.get(p);
            sb.insert(index, "(");
            sb.append(")");
        }
        
        String ret = part1;
        if (!dup.isEmpty()) {
            ret = ret + "." + sb.toString();
        }
        if ((numerator > 0 && denominator < 0)
            || (denominator > 0 && numerator < 0)) {
            ret = "-" + ret;
        }
        
        return ret;
    }
}
{% endhighlight %}