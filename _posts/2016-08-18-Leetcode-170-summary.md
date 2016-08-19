---
layout: post
title: Leetcode Problem 170 Summary
date: 2016-08-18
categories: blog
tags: [study]

---

# 题目

Design and implement a TwoSum class. It should support the following operations: add and find.

add - Add the number to an internal data structure.  
find - Find if there exists any pair of numbers which sum is equal to the value.

For example,  
add(1); add(3); add(5);  
find(4) -> true  
find(7) -> false

# 我的算法

使用HashMap记录数字和对应的次数，基本思路与 1 题相同。

# 代码

{% highlight java %}
public class TwoSum {
    Map<Integer, Integer> nums = new HashMap<>();

    // Add the number to an internal data structure.
	public void add(int number) {
	    nums.put(number, nums.containsKey(number) ? nums.get(number) + 1 : 1);
	}

    // Find if there exists any pair of numbers which sum is equal to the value.
	public boolean find(int value) {
	    for (Map.Entry<Integer, Integer> entry : nums.entrySet()) {
	        int j = value - (int)entry.getKey();
	        int i = (int)entry.getKey();
	        if ((i != j && nums.containsKey(j)) || (i == j && (int)entry.getValue() > 1))
	            return true;
	    }
	    return false;
	}
}


// Your TwoSum object will be instantiated and called as such:
// TwoSum twoSum = new TwoSum();
// twoSum.add(number);
// twoSum.find(value);
{% endhighlight %}

# discuss

{% highlight java %}
public class TwoSum {
    private List<Integer> list = new ArrayList<Integer>();
    private Map<Integer, Integer> map = new HashMap<Integer, Integer>();

    // Add the number to an internal data structure.
	public void add(int number) {
	    if (map.containsKey(number)) map.put(number, map.get(number) + 1);
	    else {
	        map.put(number, 1);
	        list.add(number);
	    }
	}

    // Find if there exists any pair of numbers which sum is equal to the value.
	public boolean find(int value) {
	    for (int i = 0; i < list.size(); i++){
	        int num1 = list.get(i), num2 = value - num1;
	        if ((num1 == num2 && map.get(num1) > 1) || (num1 != num2 && map.containsKey(num2))) return true;
	    }
	    return false;
	}
}
{% endhighlight %}