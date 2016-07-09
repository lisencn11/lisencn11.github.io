---
layout: post
title: Leetcode Problem 60 Summary
date: 2016-07-09
categories: blog
tags: [study]

---

# 题目

从1到n有n!种排列(permutation)，对各种排列进行字典排序我们可以得到。

"123"  
"132"  
"213"  
"231"  
"312"  
"321"

**输入**整型 n 和 k 。

**输出**n!种字典排列的第 k 个。

# 我的算法

假设对于第 i 位，后面有 i - 1 位，我们对于第 i 位每更换一个数字，后面都有 ( i - 1 ) ! 种排列，所以对于第 k 个排列，我们可以求k包含几个 ( i - 1 ) ! ，例如若包含一个，那么我们选择当前第二小的数字即符合字典排序。

# 代码

{% highlight java %}
public class Solution {
    List<Integer> list = new ArrayList<Integer>();
    
    public String getPermutation(int n, int k) {
        if (n < 1 || k < 1) {
            return new String();
        }
        
        for (int i = 1; i <= n; i++) {
            list.add(i);
        }
        
        String ret = permute(n, k, n);
        return ret;
    }
    
    private String permute(int n, int k, int digit) {
        String ret = null;
        if (digit == 1) {
            return String.valueOf(list.get(0));
        }
        
        int factorial = 1;
        for (int i = 1; i < digit; i++) factorial *= i;
        
        int digitToUse = (k - 1) / factorial + 1;
        int remains = (k - 1) % factorial + 1;
        int counter = 0;
        digitToUse--;
        String s = String.valueOf(list.get(digitToUse));
        list.remove(digitToUse);
        ret = s + permute(n, remains, digit-1);
        return ret;
    }
}
{% endhighlight %}