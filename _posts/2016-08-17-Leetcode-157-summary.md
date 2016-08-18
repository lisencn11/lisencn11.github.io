---
layout: post
title: Leetcode Problem 190 Summary
date: 2016-08-17
categories: blog
tags: [study]

---

# 题目

The API: int read4(char *buf) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.

Note:  
The read function will only be called once for each test case.

# 我的算法

主要考察边界条件的考虑，以及能否熟练应用新的API吧。

# 代码

{% highlight java %}
/* The read4 API is defined in the parent class Reader4.
      int read4(char[] buf); */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    public int read(char[] buf, int n) {
        int remain = n;
        int iter = 0;
        
        while (remain > 0) {
            char[] read4Buff = new char[4];
            int len = read4(read4Buff);
            for (int i = 0; i < len; i++) {
                buf[iter++] = read4Buff[i];
                if (iter == n) return iter;
            }
            if (len < 4) return iter;
        }
        
        return iter;
    }
}
{% endhighlight %}