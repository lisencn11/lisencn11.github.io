---
layout: post
title: Leetcode Problem 241 Summary
date: 2016-08-24
categories: blog
tags: [study]

---

# 题目

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.


Example 1  
Input: "2-1-1".

((2-1)-1) = 0  
(2-(1-1)) = 2  
Output: [0, 2]


Example 2  
Input: "2*3-4*5"

(2*(3-(4*5))) = -34  
((2*3)-(4*5)) = -14  
((2*(3-4))*5) = -10  
(2*((3-4)*5)) = -10  
(((2*3)-4)*5) = 10  
Output: [-34, -14, -10, -10, 10]

# 我的算法

分治算法，遍历 String ，遇到运算符的时候，将两遍分治，然后再将返回的结果 List 合并成新的 List 返回。计算出的结果用 HashMap 进行存储，也算一种 DP 吧。

犯的错误：

* 数字考虑不周全，刚开始以 start == end 作为数字，显然不对，应该是要返回的 List 为空表示是一个数字。

# 代码

{% highlight java %}
public class Solution {
    Map<Pair, List<Integer>> map;
    Set<Character> set;
    public List<Integer> diffWaysToCompute(String input) {
        char[] str = input.toCharArray();
        set = new HashSet<>();
        set.add('+');
        set.add('-');
        set.add('*');
        map = new HashMap<>();
        List<Integer> result = compute(str, 0, str.length - 1);
        return result;
    }
    
    private List<Integer> compute(char[] str, int start, int end) {
        Pair pair = new Pair(start, end);
        if (map.containsKey(pair)) return map.get(pair);
        
        List<Integer> ret = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            if (set.contains(str[i])) {
                List<Integer> list1 = compute(str, start, i - 1);
                List<Integer> list2 = compute(str, i + 1, end);
                for (int j = 0; j < list1.size(); j++) {
                    for (int k = 0; k < list2.size(); k++) {
                        if (str[i] == '+') ret.add(list1.get(j) + list2.get(k));
                        else if (str[i] == '-') ret.add(list1.get(j) - list2.get(k));
                        else ret.add(list1.get(j) * list2.get(k));
                    }
                }
            }
        }
        
        if (ret.size() == 0) {
            int res = 0;
            for (int i = start; i <= end; i++) {
                res = res * 10 + (int) (str[i] - '0');
            }
            ret.add(res);
        }
        
        map.put(pair, ret);
        return ret;
    }
    
    class Pair {
        int start;
        int end;
        public Pair(int start, int end) {
            this.start = start;
            this.end = end;
        }
    }
}
{% endhighlight %}