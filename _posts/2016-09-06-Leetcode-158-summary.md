---
layout: post
title: Leetcode Problem 336 Summary
date: 2016-09-06
categories: blog
tags: [study]

---

# 题目

The API: int read4(char *buf) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read(char *buf, int n) that reads n characters from the file.

Note:  
The read function may be called multiple times.

# 我的算法

重点在于可能多次调用，所以要记录读取的状态。

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
    
    private int bufferPt = 0;
    private int bufferCnt = 0;
    private char[] buffer = new char[4];
    public int read(char[] buf, int n) {
        int bufPt = 0;
        while (bufPt < n) {
            while (bufferPt < bufferCnt && bufPt < n) {
                buf[bufPt++] = buffer[bufferPt++];
            }
            if (bufferPt == bufferCnt) {
                bufferCnt = read4(buffer);
                bufferPt = 0;
            }
            if (bufferCnt == 0) break;
        }
        return bufPt;
    }
}
{% endhighlight %}