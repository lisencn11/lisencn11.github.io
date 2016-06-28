---
layout: post
title: Leetcode Problem 367 Summary
date: 2016-06-28
categories: blog
tags: [study]

---

# 题目

**输入**一个整型n。

**输出**整型n是否是一个平方数，即是否是另一个整型的平方

# 我的算法

初步想法，从0开始一直试直到**i * i > n**为止。但是效率低，超时。

改进，二分查找思路，将low设为0，high设为n，进行二分查找。

再次出错并查找bug：错在low, high, mid使用int，虽然之后改成使用

{% highlight java %}
long square = mid * mid;
{% endhighlight %}

进行中间存储，但是发现在mid为int的状态下，这步计算会先用一个int型存储中间结果再赋值给square，所以结果仍然会溢出，故最后将low，high，mid均改为long即可。

# 代码

{% highlight java %}
public class Solution {
    public boolean isPerfectSquare(int num) {
        long low = 0;
        long high = num;
        long square = 0;
        
        while (low <= high) {
            long mid = low + (high - low) /2;
            square = mid * mid;
            if (square < num) {
                low = mid + 1;
            } else if (square > num) {
                high = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
}
{% endhighlight %}