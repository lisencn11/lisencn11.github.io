---
layout: post
title: Leetcode Problem 190 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

Reverse bits of a given 32 bits unsigned integer.

For example, given input 43261596 (represented in binary as 00000010100101000001111010011100), return 964176192 (represented in binary as 00111001011110000010100101000000).

Follow up:  
If this function is called many times, how would you optimize it?

# 我的算法

从右到左取每一位后，从左到右加到一个新数中。

对于 Follow up ， discuss 给出的解决方案是对于每 8 位的 byte进行缓存从而提升反复计算的效果。

# 代码

{% highlight java %}
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ret = 0;
        for (int i = 0; i < 32; i++) {
            int digit = n & 1;
            n = n >>> 1;
            ret = ret << 1;
            ret = ret | digit;
        }
        
        return ret;
    }
}
{% endhighlight %}

# discuss优化

{% highlight java %}
// cache
private final Map<Byte, Integer> cache = new HashMap<Byte, Integer>();
public int reverseBits(int n) {
    byte[] bytes = new byte[4];
    for (int i = 0; i < 4; i++) // convert int into 4 bytes
        bytes[i] = (byte)((n >>> 8*i) & 0xFF);
    int result = 0;
    for (int i = 0; i < 4; i++) {
        result += reverseByte(bytes[i]); // reverse per byte
        if (i < 3)
            result <<= 8;
    }
    return result;
}

private int reverseByte(byte b) {
    Integer value = cache.get(b); // first look up from cache
    if (value != null)
        return value;
    value = 0;
    // reverse by bit
    for (int i = 0; i < 8; i++) {
        value += ((b >>> i) & 1);
        if (i < 7)
            value <<= 1;
    }
    cache.put(b, value);
    return value;
}
{% endhighlight %}