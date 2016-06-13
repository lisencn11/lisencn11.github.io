---
layout: post
title: Leetcode Problem 341 Summary
date: 2016-06-13
categories: blog
tags: [study]

---

# 题目

**输入**一个整型嵌套列表。

**输出**将原链表“展平”，即变成一维列表。

输入：[[1,1],2,[1,1]];

输出：[1,1,2,1,1]

写一个Iterator实现以上功能。

# 我的算法

选择使用栈(Stack)，但是next()和hasNext()编写出错。错在通过栈空来判断hasNext()，但是存在栈不空hasNext()仍返回false的情况，如:[[]]。

# 代码

```java

/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    private Stack<NestedInteger> nestedIntegerStack = new Stack<NestedInteger>();

    public NestedIterator(List<NestedInteger> nestedList) {
        for (int i = nestedList.size() - 1; i >= 0; i--) {
            nestedIntegerStack.push(nestedList.get(i));
        }
    }

    @Override
    public Integer next() {
        return nestedIntegerStack.pop().getInteger();
    }

    @Override
    public boolean hasNext() {
        while (!nestedIntegerStack.empty()) {
            NestedInteger nestedInteger = nestedIntegerStack.peek();
            if (nestedInteger.isInteger()) {
                return true;
            }
            nestedIntegerStack.pop();
            List<NestedInteger> list = nestedInteger.getList();
            for (int i = list.size() - 1; i >= 0; i--) {
                nestedIntegerStack.push(list.get(i));
            }
        }
        
        return false;
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */

```
