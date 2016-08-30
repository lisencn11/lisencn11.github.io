---
layout: post
title: Leetcode Problem 372 Summary
date: 2016-07-11
categories: blog
tags: [study]

---

# 题目

**输入**一个正整型a，一个正整型数组b，b表示一个很大的正整数，所以用数组表示。

**输出**a<sup>b</sup> mod 1337的结果。

如：  
>a = 2  
b = [1,0]

>Result: 1024

# 我的算法

根据公式推导若 a<sup>b</sup> == 1337x + y 那么 a<sup>b+1</sup> == a(1337x + y) == 1337ax + ay。所以可以发现每次求出mod值后乘以a再mod即可求出下次mod值。mod的值组成一个圈，最大长度为1335（最大长度中0和1不会出现）。

使用long类型避免相乘时溢出，使用BigInteger以适应超大指数，数组保存环，序号表示当前摩值，值表示下一个摩值。

# 代码

{% highlight java %}
import java.math.BigInteger;

public class Solution {
    private static final long BASE = 1337;
    
    public int superPow(int a, int[] b) {
        if (a <= 0 || b == null || b.length == 0) {
            return -1;
        }
        
        long longA = (long)a;
        long mod0 = longA % BASE, mod1 = (mod0 * longA) % BASE;
        if (mod0 == mod1) {
            return (int)mod0;
        }
        int counter0 = 0;
        long[] modArray = new long[(int)BASE];
        long modStart = mod0;
        
        while (modArray[(int)mod0] != mod1) {
            modArray[(int)mod0] = mod1;
            mod0 = mod1;
            mod1 = (mod0 * longA) % BASE;
            counter0++;
        }
        
        int modLen = counter0;

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < b.length; i++) {
            sb.append(b[i]);
        }
        
        BigInteger bi0 = new BigInteger(sb.toString());
        BigInteger bi1 = new BigInteger(String.valueOf(modLen));
        
        int steps = bi0.subtract(new BigInteger("1")).mod(bi1).intValue();
        long modIter = modStart;
        for (int i = 0; i < steps; i++) {
            modIter = modArray[(int)modIter];
        }
        
        return (int)modIter;
    }
}
{% endhighlight %}

# 二刷

二刷使用的模的一个规律 a*b mod c = (a mod c) * (b mod c)，所以我们对于 a<sup>12345</sup> mod 1337，可以先算 a<sup>5</sup> mod 1337 再算 a<sup>12340</sup> mod 1337，a<sup>12340</sup> mod 1337 又可以先算 res = a<sup>1234</sup> mod 1337，再算 res<sup>10</sup> mod 1337，所以时间复杂度为 O(n)。

{% highlight java %}
public class Solution {
    static final int BASE = 1337;
    
    public int superPow(int a, int[] b) {
        if (b == null || b.length == 0) return 1;
        return superPowWrapper(a, b, b.length - 1);
    }
    
    private int superPowWrapper(int a, int[] b, int end) {
        if (end == 0) return powCalculate(a, b[end]);
        
        return (powCalculate(superPowWrapper(a, b, end - 1), 10) * powCalculate(a, b[end])) % BASE;
    }
    
    private int powCalculate(int a, int k) {
        a %= BASE;
        int result = 1;
        for (int i = 0; i < k; i++) result = (result * a) % BASE;
        return result;
    }
}
{% endhighlight %}