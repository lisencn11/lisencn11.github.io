---
layout: post
title: Leetcode Problem 93 Summary
date: 2016-07-12
categories: blog
tags: [study]

---

# 题目

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:  
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter)

# 我的算法

递归计算，假设总共 n 位，取后 i 位，计算出前 n - i 位有可能的集合，然后将后 i 位加入即可。

需要检查当前取的是否在0-255间，取后剩下的位数是否符合规则，比如如果只剩下2个域却有7位则剩下的计算可跳过。

# 代码

{% highlight java %}
public class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> ret = new ArrayList<>();
        if (s == null || s.length() < 4 || s.length() > 12) {
            return ret;
        }
        
        List<StringBuilder> sb = restoreIpAddressesHelper(s, s.length(), 4);
        
        for (StringBuilder iter : sb) {
            ret.add(iter.toString());
        }
        return ret;
    }
    
    private List<StringBuilder> restoreIpAddressesHelper(String s, int end, int part) {
        List<StringBuilder> ret = new ArrayList<>();
        if (part == 1) {
            int num = translate(s, 0, end);
            if (num != -1) {
                ret = new ArrayList<>();
                StringBuilder sb = new StringBuilder();
                sb.append(num);
                ret.add(sb);
                return ret;
            } else {
                return new ArrayList<>();
            }
        }
        
        for (int i = end - 1; i >= end - 3; i--) {
            if (!validAddress(i, part - 1)) {
                continue;
            }
            int num = translate(s, i, end);
            if (num != -1) {
                List<StringBuilder> sb = restoreIpAddressesHelper(s, i, part - 1);
                for (StringBuilder iter : sb) {
                    iter.append(".");
                    iter.append(num);
                }
                ret.addAll(sb);
            } 
        }
        
        return ret;
    }
    
    private int translate(String s, int start, int end) {
        char[] c = s.toCharArray();
        if ((end - start > 1) && (c[start] == '0')) {
            return -1;
        }
        int ret = 0;
        for (int i = start; i < end; i++) {
            ret = ret * 10 + (c[i] - '0');
        }
        
        if (ret >= 0 && ret <= 255) {
            return ret;
        } else {
            return -1;
        }
    }
    
    private boolean validAddress(int len, int part) {
        if ((len <= part * 3) && (len >= part)) {
            return true;
        } else {
            return false;
        }
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    static final int IP_PARTS = 4;
    static final int IP_SEGMENT = 3;
    
    public List<String> restoreIpAddresses(String s) {
        List<String> result = new ArrayList<>();
        restoreHelper(result, s, "", 0);
        return result;
    }
    
    private void restoreHelper(List<String> result, String s, String curr, int part) {
        if (part == IP_PARTS && s.equals("")) {
            result.add(curr.substring(0, curr.length() - 1));
            return;
        }
        if (part == IP_PARTS || s.equals("")) return;
        if (((IP_PARTS - part) * IP_SEGMENT) < s.length()) return;
        if (s.charAt(0) == '0') {
            restoreHelper(result, s.substring(1), curr + "0.", part + 1);
            return;
        }
        
        for (int i = 1; i <= 3 && i <= s.length(); i++) {
            String sub = s.substring(0, i);
            int ip = Integer.parseInt(sub);
            if (ip <= 0 || ip > 255) continue;
            restoreHelper(result, s.substring(i), curr + sub + ".", part + 1);
        }
    }
}
{% endhighlight %}