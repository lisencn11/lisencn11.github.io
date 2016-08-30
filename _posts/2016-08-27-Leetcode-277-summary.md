---
layout: post
title: Leetcode Problem 277 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a function int findCelebrity(n), your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return -1.

# 我的算法

从 0 开始遍历，如果 0 认识 i 但不认识 i 以前的，说明 i 以前的肯定不是名人， i 可能是名人，然后从 i 开始遍历，过程和 0 一样，直到找到最后一个可能是名人的人。

check 是否所有人都认识我们找到的这个可能是名人的人。

# 代码

{% highlight java %}
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        if (n == 0) return -1;
        if (n == 1) return 0;
        
        int p1 = 0;

        while (p1 < n) {
            int p2 = p1 + 1;
            while (p2 < n) {
                if (knows(p1, p2)) {
                    p1 = p2;
                    break;
                }
                p2++;
            }
            
            if (p2 == n) {
                for (int i = p1 - 1; i >= 0; i--) {
                    if (knows(p1, i)) return -1;
                }
                for (int i = 0; i < n; i++) {
                    if (i != p1 && !knows(i, p1)) return -1;
                }
                return p1;
            }
        }
        return -1;
    }
}
{% endhighlight %}

# discuss

相同思路，代码结构更好，调理更清晰。

{% highlight java %}
public class Solution extends Relation {
    public int findCelebrity(int n) {
        int candidate = 0;
        for(int i = 1; i < n; i++){
            if(knows(candidate, i))
                candidate = i;
        }
        for(int i = 0; i < n; i++){
            if(i != candidate && (knows(candidate, i) || !knows(i, candidate))) return -1;
        }
        return candidate;
    }
}
{% endhighlight %}