---
layout: post
title: Leetcode Problem 66 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

# 我的算法

我选择使用 Stack 以确保能够更简单的处理各种边界条件，实际上最简单的是判断是否某一位非 9 ，遇到非 9 可以直接返回结果否则新申请一个数组置为 1000... 即可。

# 代码

{% highlight java %}
public class Solution {
    public int[] plusOne(int[] digits) {
        Stack<Integer> stack = new Stack<>();
        int carry = 1;
        
        for (int i = digits.length - 1; i >= 0; i--) {
            int digit = digits[i] + carry;
            if (digit == 10) {
                stack.push(0);
            } else {
                carry = 0;
                stack.push(digit);
            }
        }
        if (carry == 1) stack.push(1);
        
        int[] ret = new int[stack.size()];
        int iter = 0;
        while (!stack.isEmpty())
            ret[iter++] = stack.pop();
            
        return ret;
    }
}
{% endhighlight %}

### 更高效

{% highlight java %}
public class Solution {
    public int[] plusOne(int[] digits) {
        
    	int n = digits.length;
    	for(int i=n-1; i>=0; i--) {
    		if(digits[i] < 9) {
       		digits[i]++;
          	return digits;
       	}
        
        	digits[i] = 0;
    	}
    
    	int[] newNumber = new int [n+1];
    	newNumber[0] = 1;
    
    	return newNumber;
	}
}
{% endhighlight %}