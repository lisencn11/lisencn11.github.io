---
layout: post
title: Leetcode Problem 380 Summary
date: 2016-08-07
categories: blog
tags: [study]

---

# 题目

设计一个数据结构，对下列操作支持 O(1) 时间复杂度的操作：

1. insert(val) 如果 val 不存在就插入
2. remove(val) 如果 val 存在就删除
3. getRandom 从当前元素集合中，以每个元素相同的概率返回一个元素

用例：  
// Init an empty set.  
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.  
randomSet.insert(1);

// Returns false as 2 does not exist in the set.  
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].  
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.  
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].  
randomSet.remove(1);

// 2 was already in the set, so return false.  
randomSet.insert(2);

// Since 1 is the only number in the set, getRandom always return 1.  
randomSet.getRandom();

# 我的算法

对于插入和删除的 O(1) 操作可以简单的使用哈希表，而对于取随机数的 O(1) 操作可以使用数组实现，于是产生想法将两者结合起来，思路卡在如何以 O(1) 的时间从 ArrayList 中删除一个元素，解决办法是将这个元素和ArrayList尾部元素交换后删除尾部元素即可不对其他元素产生影响。

# 代码

{% highlight java %}
import java.util.Random;
public class RandomizedSet {
    Map<Integer, Integer> map;
    List<Integer> list;
    Random rand;
    
    /** Initialize your data structure here. */
    public RandomizedSet() {
        map = new HashMap<>();
        list = new ArrayList<>();
        rand = new Random();
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.containsKey(val)) return false;
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) return false;
        int index = map.get(val);
        if (index < list.size() - 1) {
            int temp = list.get(index);
            list.set(index, list.get(list.size() - 1));
            list.set(list.size() - 1, temp);
            map.put(list.get(index), index);
        }
        list.remove(list.size() - 1);
        map.remove(val);
        return true;
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        return list.get(rand.nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
{% endhighlight %}