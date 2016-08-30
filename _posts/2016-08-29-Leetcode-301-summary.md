---
layout: post
title: Leetcode Problem 301 Summary
date: 2016-08-29
categories: blog
tags: [study]

---

# 题目

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).

Examples:  
"()())()" -> ["()()()", "(())()"]  
"(a)())()" -> ["(a)()()", "(a())()"]  
")(" -> [""]

# 我的算法

这道题刚开始想不出什么算法，然后看 tag 告诉我是 DFS 或者 BFS，依然看不出来。看到 discuss 才明白这道题压根没有算法，所谓 BFS 不过是把相同长度的字符串的所有可能遍历出来检查是否合法，再次提醒我，以后面试要先从最好想最直接的算法一步一步优化，而不是一开始就想要次优解甚至最优解。

# 代码

{% highlight java %}
public class Solution {
    public List<String> removeInvalidParentheses(String s) {
        Queue<String> queue = new LinkedList<>();
        queue.offer(s);
        Set<String> set = new HashSet<>();
        set.add(s);
        List<String> list = new ArrayList<>();
        
        while (!queue.isEmpty()) {
            int len = queue.size();
            boolean found = false;
            for (int i = 0; i < len; i++) {
                String curr = queue.poll();
                if (isValid(curr)) {
                    found = true;
                    list.add(curr);
                }
                if (found) continue;
                for (int j = 0; j < curr.length(); j++) {
                    if (curr.charAt(j) == '(' || curr.charAt(j) == ')') {
                        String sub = curr.substring(0, j) + curr.substring(j + 1);
                        if (!set.contains(sub)) {
                            set.add(sub);
                            queue.offer(sub);
                        }
                    }
                }
            }
            if (found) break;
        }
        return list;
    }
    
    private boolean isValid(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') count++;
            else if (s.charAt(i) == ')') count--;
            
            if (count < 0) return false;
        }
        
        return count == 0;
    }
}
{% endhighlight %}