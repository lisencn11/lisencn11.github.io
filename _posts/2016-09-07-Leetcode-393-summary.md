---
layout: post
title: Leetcode Problem 393 Summary
date: 2016-09-07
categories: blog
tags: [study]

---

# 题目

A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

1. For 1-byte character, the first bit is a 0, followed by its unicode code.
2. For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
This is how the UTF-8 encoding would work:

![](https://lisencn11.github.io/img/problem393.png)

Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

Note:  
The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

Example 1:

data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.

Return true.  
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.

Example 2:

data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.

Return false.

The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.  
The next byte is a continuation byte which starts with 10 and that's correct.  
But the second continuation byte does not start with 10, so it is invalid.

# 我的算法

bit manipulation，根据当前位判断尾部长度

# 代码

{% highlight java %}
public class Solution {
    public boolean validUtf8(int[] data) {
        for (int i = 0; i < data.length; ) {
            int len = getTailLen(data[i]);
            
            if (len == 0) {
                i++;
                continue;
            }
            if (len == -1) return false;
            i++;
            while (i < data.length && len > 0) {
                if (data[i] < 0x80 || data[i] > 0xBF) return false;
                len--;
                i++;
            }
            if (len > 0) return false;
        }
        return true;
    }
    
    private int getTailLen(int data) {
        if (data <= 0x7F) return 0;
        else if (data <= 0xBF) return -1;
        else if (data <= 0xDF) return 1;
        else if (data <= 0xEF) return 2;
        else if (data <= 0xF7) return 3;
        else return -1;
    }
}
{% endhighlight %}