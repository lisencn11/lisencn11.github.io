---
layout: post
title: Leetcode Problem 284 Summary
date: 2016-08-27
categories: blog
tags: [study]

---

# 题目

Given an Iterator class interface with methods: next() and hasNext(), design and implement a PeekingIterator that support the peek() operation -- it essentially peek() at the element that will be returned by the next call to next().

Here is an example. Assume that the iterator is initialized to the beginning of the list: [1, 2, 3].

Call next() gets you 1, the first element in the list.

Now you call peek() and it returns 2, the next element. Calling next() after that still return 2.

You call next() the final time and it returns 3, the last element. Calling hasNext() after that should return false.

Hint:

1. Think of "looking ahead". You want to cache the next element.
2. Is one variable sufficient? Why or why not?
3. Test your design with call order of peek() before next() vs next() before peek().
4. For a clean implementation, check out Google's guava library source code.

Follow up: How would you extend your design to be generic and work with all types, not just integer?

# 我的算法

每次 next() 操作的时候都缓存下一个数。

# 代码

{% highlight java %}
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html
class PeekingIterator implements Iterator<Integer> {
    Iterator<Integer> iterator;
    Integer top = -1;
    boolean end = true;

	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    this.iterator = iterator;
	    if (iterator.hasNext()) {
	        top = iterator.next();
	        end = false;
	    }
	}

    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return top;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    int ret = top;
	    if (iterator.hasNext()) top = iterator.next();
	    else end = true;
	    return ret;
	}

	@Override
	public boolean hasNext() {
	    return !end;
	}
}
{% endhighlight %}