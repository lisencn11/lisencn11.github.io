---
layout: post
title: Leetcode Problem 254 Summary
date: 2016-08-25
categories: blog
tags: [study]

---

# 题目

Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;  
  = 2 x 4.  
Write a function that takes an integer n and return all possible combinations of its factors.

Note:   
You may assume that n is always positive.  
Factors should be greater than 1 and less than n.  
Examples:
   
input: 1  
output:   
[]
  
input: 37  
output:   
[]

input: 12  
output:  
[  
  [2, 6],  
  [2, 2, 3],  
  [3, 4]  
]

input: 32  
output:  
[  
  [2, 16],  
  [2, 2, 8],  
  [2, 2, 2, 4],  
  [2, 2, 2, 2, 2],  
  [2, 4, 4],  
  [4, 8]  
]

# 我的算法

普通的 backtrack 解法，效率位于第二梯队。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> list = helper(n, 2);
        list.remove(list.size() - 1);
        return list;
    }
    
    private List<List<Integer>> helper(int n, int start) {
        List<List<Integer>> ret = new ArrayList<>();
        if (n == 1) {
            ret.add(new ArrayList<>());
        }
        
        for (int i = start; i <= n; i++) {
            if (n % i == 0) {
                List<List<Integer>> list = helper(n / i, i);
                for (List<Integer> iter : list) {
                    iter.add(i);
                }
                ret.addAll(list);
            }
        }
        
        return ret;
    }
}
{% endhighlight %}

# discuss

同样是 backtrack ，但思路略有不同，这个思路能够实现对寻找因子时的剪枝。

{% highlight java %}
public List<List<Integer>> getFactors(int n) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if (n <= 3) return result;
    helper(n, -1, result, new ArrayList<Integer>());
    return result; 
}

public void helper(int n, int lower, List<List<Integer>> result, List<Integer> cur) {
    if (lower != -1) {
        cur.add(n);
        result.add(new ArrayList<Integer>(cur));
        cur.remove(cur.size() - 1);
    }
    int upper = (int) Math.sqrt(n);
    for (int i = Math.max(2, lower); i <= upper; ++i) {
        if (n % i == 0) {
            cur.add(i);
            helper(n / i, i, result, cur);
            cur.remove(cur.size() - 1);
        }
    }
}
{% endhighlight %}