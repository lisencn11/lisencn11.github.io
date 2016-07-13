---
layout: post
title: Leetcode Problem 215 Summary
date: 2016-07-13
categories: blog
tags: [study]

---

# 题目

**输入**一个无序整型数组。

**输出**这个数组中第n大的元素。

如：  
>Given [3,2,1,5,6,4] and k = 2, return 5.

# 我的算法

普通算法是先排序，然后直接返回，时间复杂度为O(nlogn)。

可以使用堆，即优先队列，时间复杂度也为O(nlogn)。

基于快排的快速选择算法，从数组中选择任意一个元素，进行一次划分，元素左方都小于它，右方都大于它。若此时元素序号小于length - k，表示目标元素在它右方，反之则在其左方。最坏时间复杂度为O(n<sup>2</sup>)，但是平均时间复杂度可证明为O(n)。

# 代码

{% highlight java %}
public class Solution {
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    public int findKthLargest(int[] nums, int k) {
        boolean found = false;
        int start = 0;
        int end = nums.length - 1;
        
        while (!found) {
            int pivot = nums[start];
            int low = start;
            int high = end;
            int PivotIndex = 0;
            int rank = 0;
            while (low < high) {
                while (low < high && nums[high] >= pivot) high--;
                if (low < high) nums[low++] = nums[high];
                while (low < high && nums[low] < pivot) low++;
                if (low < high) nums[high--] = nums[low];
            }
            nums[low] = pivot;
            int pivotIndex = low;
            
            rank = nums.length - pivotIndex;
            if (rank == k) {
                return nums[pivotIndex];
            } else if (rank < k) {
                end = pivotIndex - 1;
            } else {
                start = pivotIndex + 1;
            }
        }
        return -1;
    }
}
{% endhighlight %}
