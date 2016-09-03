---
layout: post
title: Leetcode Problem 271 Summary
date: 2016-09-02
categories: blog
tags: [study]

---

# 题目

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:

string encode(vector<string> strs) {  
  // ... your code  
  return encoded_string;  
}

Machine 2 (receiver) has the function:

vector<string> decode(string s) {  
  //... your code  
  return strs;  
}

So Machine 1 does:

string encoded_string = encode(strs);

and Machine 2 does:

vector<string> strs2 = decode(encoded_string);

strs2 in Machine 2 should be the same as strs in Machine 1.

Implement the encode and decode methods.

Note:  
* The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.
* Do not rely on any library method such as eval or serialize methods. You should implement your own encode/decode algorithm.

# 我的算法

方法是在每个字符串见面加上 length + "/" 首先我们可以保证收到字符串的最前面的 "/" 是我们加入的，那么前面的长度能够让我们直接跳到下一个 String ，从而避免和 String 中可能包含的数字和 "/" 相混淆。

# 代码

{% highlight java %}
public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for (String s : strs) {
            int len = s.length();
            sb.append(len).append("/").append(s);
        }
        return sb.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        int iter = 0;
        List<String> list = new ArrayList<>();
        while (iter < s.length()) {
            int slash = s.indexOf("/");
            int len = Integer.valueOf(s.substring(iter, slash));
            list.add(s.substring(slash + 1, slash + len + 1));
            s = s.substring(slash + len + 1);
        }
        return list;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(strs));
{% endhighlight %}