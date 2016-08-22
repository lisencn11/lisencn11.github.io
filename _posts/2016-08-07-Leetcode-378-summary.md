---
layout: post
title: Leetcode Problem 378 Summary
date: 2016-08-07
categories: blog
tags: [study]

---

# 题目

一个整型矩阵 int[][] ，其每一行和每一列都是升序排列，找到这个矩阵中第 k 小的元素

例如：  
matrix = [  
   [ 1,  5,  9],  
   [10, 11, 13],  
   [12, 13, 15]  
],  
k = 8,

return 13.

# 我的算法

最直接的算法是读入所有元素，排序后直接输出第 k 小的元素，空间复杂度为 O(n) ，时间复杂度为 O(nlogn)。

另一算法的思路是我们在一个最小堆 PriorityQueue 中存储每一列的最小值，初始化为第一行的元素，从中取出一个元素，就是当前的最小值，然后将与其同列的下一行元素放进去，就可以去下一个最小值，重复 k - 1 次后堆顶就是第 k 小的元素。空间复杂度为 O(sqrt(n)) ，时间复杂度为 O(sqrt(n) + k)

# 代码

{% highlight java %}
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return -1;
        
        Queue<Tuple> queue = new PriorityQueue<>();
        int bound = matrix.length < k ? matrix.length : k;
        for (int i = 0; i < bound; i++) queue.offer(new Tuple(0, i, matrix[0][i]));
        for (int i = 0; i < k - 1; i++) {
            Tuple curr = queue.poll();
            if (curr.x == bound - 1) continue;
            queue.offer(new Tuple(curr.x + 1, curr.y, matrix[curr.x + 1][curr.y]));
        }
        return queue.poll().val;
    }
}

class Tuple implements Comparable<Tuple> {
    int x, y, val;
    
    public Tuple(int x, int y, int val) {
        this.x = x;
        this.y = y;
        this.val = val;
    }
    
    @Override
    public int compareTo(Tuple tuple) {
        return this.val - tuple.val;
    }
}
{% endhighlight %}

# 二刷

二刷采取了不同的算法，二分查找，因为这道是新题，没有足够数据看不出两者速度差异。

{% highlight java %}
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int row = matrix.length, col = matrix[0].length;
        int low = matrix[0][0], high = matrix[row - 1][col - 1];
        while (low < high) {
            int mid = low + (high - low) / 2;
            int num = 0;
            for (int i = 0; i < row; i++) {
                int pos = Arrays.binarySearch(matrix[i], mid);
                if (pos > 0) {
                    while (pos < (col - 1) && matrix[i][pos + 1] == mid) pos++;
                }
                pos = pos < 0 ? -(pos + 1) : pos + 1;
                
                num += pos;
            }
            if (num < k) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}
{% endhighlight %}