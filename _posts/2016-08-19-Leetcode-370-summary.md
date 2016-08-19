---
layout: post
title: Leetcode Problem 370 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

Assume you have an array of length n initialized with all 0's and are given k update operations.

Each operation is represented as a triplet: [startIndex, endIndex, inc] which increments each element of subarray A[startIndex ... endIndex] (startIndex and endIndex inclusive) with inc.

Return the modified array after all k operations were executed.

Example:

Given:

    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
    
Explanation:

Initial state:  
[ 0, 0, 0, 0, 0 ]

After applying operation [1, 3, 2]:  
[ 0, 2, 2, 2, 0 ]

After applying operation [2, 4, 3]:  
[ 0, 2, 5, 5, 3 ]

After applying operation [0, 2, -2]:  
[-2, 0, 3, 5, 3 ]

Hint:

* Thinking of using advanced data structures? You are thinking it too complicated.
* For each update operation, do you really need to update all elements between i and j?
* Update only the first and end element is sufficient.
* The optimal time complexity is O(k + n) and uses O(1) extra space.

# 我的算法

最直接的算法是每次都对从 startIndex 到 endIndex 的所有元素进行更新，结果是超时。

思考一定要对所有元素都进行更新吗？我们可以只对收尾（尾为exclusive）进行更新，更新值为之后元素的偏移量。

# 代码

{% highlight java %}
public class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        if (updates == null || updates.length == 0) return new int[length];
        int[] array = new int[length + 1];
        int[] result = new int[length];
        
        for (int i = 0; i < updates.length; i++) {
            array[updates[i][0]] += updates[i][2];
            array[updates[i][1] + 1] -= updates[i][2];
        }
        
        result[0] = array[0];
        for (int i = 1; i < result.length; i++) {
            result[i] = result[i - 1] + array[i];
        }
        
        return result;
    }
}
{% endhighlight %}