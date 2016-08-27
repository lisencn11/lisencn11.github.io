---
layout: post
title: Leetcode Problem 251 Summary
date: 2016-08-26
categories: blog
tags: [study]

---

# 题目

Implement an iterator to flatten a 2d vector.

For example,  
Given 2d vector =

[  
  [1,2],  
  [3],  
  [4,5,6]  
]

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,2,3,4,5,6].

Hint:

1. How many variables do you need to keep track?
2. Two variables is all you need. Try with x and y.
3. Beware of empty rows. It could be the first few rows.
4. To write correct code, think about the invariant to maintain. What is it?
5. The invariant is x and y must always point to a valid point in the 2d vector. Should you maintain your invariant ahead of time or right when you need it?
6. Not sure? Think about how you would implement hasNext(). Which is more complex?
7. Common logic in two different places should be refactored into a common method.

# 我的算法

两个 pointer 追踪在 List<List> 和 List 的位置。

# 代码

{% highlight java %}
public class Vector2D implements Iterator<Integer> {
    List<List<Integer>> vector = null;
    List<Integer> list = null;
    int vectorIndex = 0;
    int listIndex = 0;
    int next = 0;

    public Vector2D(List<List<Integer>> vec2d) {
        if (vec2d == null || vec2d.size() == 0) return;
        
        vector = vec2d;
        list = vector.get(vectorIndex);
        vectorIndex++;
    }

    @Override
    public Integer next() {
        return next;
    }

    @Override
    public boolean hasNext() {
        if (vector == null) return false;
        if (listIndex < list.size()) {
            next = list.get(listIndex++);
            return true;
        } else {
            while (vectorIndex < vector.size()) {
                list = vector.get(vectorIndex);
                if (!list.isEmpty()) {
                    listIndex = 0;
                    next = list.get(listIndex++);
                    vectorIndex++;
                    return true;
                }
                vectorIndex++;
            }
            return false;
        }
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i = new Vector2D(vec2d);
 * while (i.hasNext()) v[f()] = i.next();
 */
{% endhighlight %}