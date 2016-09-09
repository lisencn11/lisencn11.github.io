---
layout: post
title: Leetcode Problem 282 Summary
date: 2016-09-08
categories: blog
tags: [study]

---

# 题目

Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Examples:   
"123", 6 -> ["1+2+3", "1*2*3"]   
"232", 8 -> ["2*3+2", "2+3*2"]  
"105", 5 -> ["1*0+5","10-5"]  
"00", 0 -> ["0+0", "0-0", "0*0"]  
"3456237490", 9191 -> []

# 我的算法

主要问题是乘法的处理，我们需要记录前一个数的状态。

# 代码

{% highlight java %}
public class Solution {
    public List<String> addOperators(String num, int target) {
        if (num == null || num.length() == 0) return new ArrayList<>();
        List<String> list = new ArrayList<>();
        helper(num, "", list, 0, 0, 0, target);
        return list;
    }
    
    private void helper(String num, String path, List<String> list, int pos, long result, long multiply, int target) {
        if (pos == num.length()) {
            if (result == target) {
                list.add(path);
            }
            return;
        }
        
        for (int i = pos; i < num.length(); i++) {
            if (i != pos && num.charAt(pos) == '0') break;
            String currS = num.substring(pos, i + 1);
            long currL = Long.parseLong(currS);
            if (pos == 0) {
                helper(num, currS, list, i + 1, currL, currL, target);
            } else {
                helper(num, path + "+" + currS, list, i + 1, result + currL, currL, target);
                helper(num, path + "-" + currS, list, i + 1, result - currL, -currL, target);
                helper(num, path + "*" + currS, list, i + 1, result - multiply + multiply * currL, multiply * currL, target);
            }
        }
    }
}
{% endhighlight %}