---
layout: post
title: Leetcode Problem 371 Summary
date: 2016-07-06
categories: blog
tags: [study]

---

# 题目

**输入**整型a和b。

**输出**a和b的加和，要求不能使用运算符+,-,++,--。

# 我的算法

使用位运算：**与运算**可以得到两个数均为1的位，**异或运算**可以得到有一位是1的位，我们可以通过先使用**与运算**保存下两位均为1的位，再使用**异或运算**对两位中有一位是1的进行加和存入a中，最后将之前保存的与运算结果左移一位得到进位，将左移结果存入b中，将异或结果a和与运算并左移的结果b重复上述步骤直到进位为0，即b为0.

# 代码

{% highlight java %}
public class Solution {
    public int getSum(int a, int b) {
        int carry = 0;
        
        while (b != 0) {
            carry = a & b;
            a = a ^ b;
            b = carry << 1;
        }
        
        return a;
    }
}
{% endhighlight %}