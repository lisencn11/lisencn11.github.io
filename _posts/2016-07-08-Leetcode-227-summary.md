---
layout: post
title: Leetcode Problem 227 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**输入**一个字符串类型的表达式，除了数字还有 '+' '-' '*' '/' ' '。

**输出**表达式的计算结果。

例如  
>"3+2*2" = 7

>" 3/2 " = 1

>" 3+5 / 2 " = 5

# 我的算法

为了防止大数运算使用BigInteger，两个List，一个存储数，一个存储运算符，遍历两遍运算符List，第一遍计算乘除法，第二遍计算加减法。

此题测试用例没有涉及大数，结果BigInteger导致程序变慢。

# 代码

{% highlight java %}
import java.math.BigInteger;

public class Solution {
    public int calculate(String s) {
        s = s.replaceAll(" ", "");
        char[] charArray = s.toCharArray();
        
        List<Character> cList = new ArrayList<Character>();
        List<BigInteger> bList = new ArrayList<BigInteger>();
        List<BigInteger> numList = new ArrayList<BigInteger>();
        List<Character> opList = new ArrayList<Character>();

        for (int i = 0; i < charArray.length; i++) {
            if (charArray[i] == '+' || charArray[i] == '-' || charArray[i] == '*' || charArray[i] == '/') {
                cList.add(new Character(charArray[i]));
                charArray[i] = ' ';
            }
        }
        
        String[] strs = String.valueOf(charArray).split(" ");
        for (String str : strs) {
            BigInteger bi = new BigInteger(str);
            bList.add(bi);
        }
        
        BigInteger temp = bList.get(0);
        for (int i = 0; i < cList.size(); i++) {
            if (cList.get(i) == '*') {
                temp = temp.multiply(bList.get(i + 1));
            } else if (cList.get(i) == '/') {
                temp = temp.divide(bList.get(i + 1));
            } else {
                numList.add(temp);
                temp = bList.get(i + 1);
                opList.add(cList.get(i));
            }
        }
        numList.add(temp);
        
        BigInteger result = numList.get(0);
        for (int i = 0; i < opList.size(); i++) {
            if (opList.get(i) == '+') {
                result = result.add(numList.get(i + 1));
            } else if (opList.get(i) == '-') {
                result = result.subtract(numList.get(i + 1));
            }
        }
        
        return result.intValue();
    }
}
{% endhighlight %}