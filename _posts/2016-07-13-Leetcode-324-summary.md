---
layout: post
title: Leetcode Problem 324 Summary
date: 2016-07-13
categories: blog
tags: [study]

---

# 题目

**输入**一个无序整型数组。

**输出**重排序为nums[0] < nums[1] > nums[2] < nums[3]...。

如：  
>(1) Given nums = [1, 5, 1, 1, 6, 4], one possible answer is [1, 4, 1, 5, 1, 6].   
(2) Given nums = [1, 3, 2, 2, 3, 1], one possible answer is [2, 3, 1, 3, 1, 2].

# 我的算法

算法分为三步：

1. 对数组进行排序
2. 使用两个pointer，分别从尾部和中间向前遍历

# 代码

{% highlight java %}
public class Solution {
    public void wiggleSort(int[] nums) {
        if (nums == null || nums.length == 0) return;
        
        Arrays.sort(nums);
        int[] temp = new int[nums.length];
        int low = (nums.length - 1) / 2, high = nums.length - 1, iter = 0;
        while (low >= 0 && (high > ((nums.length - 1) / 2))) {
            temp[iter++] = nums[low--];
            temp[iter++] = nums[high--];
        }
        if (low == 0) {
            temp[iter] = nums[low];
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }
    
}
{% endhighlight %}

# 改进

上面的代码是O(nlogn)的时间复杂度和O(n)的空间复杂度。

题目要求能够改进到O(n)时间复杂度和O(1)空间复杂度。

我们之前的算法的实质是利用了中位数，如果要改进算法，我们需要引进O(n)时间复杂度内找到中位数的算法。

LeetCode 215 题中我们有该解法。

通过O(n)的时间复杂度我们能够找到中位数，接下来我们需要在O(n)的时间复杂度和的O(1)空间复杂度进行数组的转换，我们需要使用两个思想：**[index映射](https://discuss.leetcode.com/topic/32929/o-n-o-1-after-median-virtual-indexing)**和**[three-way partition](https://en.wikipedia.org/wiki/Dutch_national_flag_problem#Pseudocode)**。

**three-way partition**能够实现将数组分为三块，最左边是小于中位数的，最右边是大于中位数的，中间是中位数，而我们基于快排的查找不能做到将中位数在中间连在一起。

**index映射**是指对于此题，我们并不想将小于等于中位数的都移动到左边，而是移动到数组序号为偶数的位置；将大于等于中位数的都移动到数组序号为奇数的位置，我们只需要将序号做一个映射，就能够做到在不改变**three-way partition**算法的情况下，实现原算法结果的左半边（假设大于等于0）依次放入奇数序号的位置，愿算法结果的右半边依次放入偶数序号的位置。

# 改进后代码

注：findKthLargest()的代码可以在215题的博客中找到并直接使用。

{% highlight java %}
public class Solution {
    public void wiggleSort(int[] nums) {
        int median = findKthLargest(nums, (nums.length + 1) / 2);
        int n = nums.length;

        int left = 0, i = 0, right = n - 1;

        while (i <= right) {

            if (nums[newIndex(i,n)] > median) {
                swap(nums, newIndex(left++,n), newIndex(i++,n));
            }
            else if (nums[newIndex(i,n)] < median) {
                swap(nums, newIndex(right--,n), newIndex(i,n));
            }
            else {
                i++;
            }
        }
    }

    private int newIndex(int index, int n) {
        return (1 + 2*index) % (n | 1);
    }
}
{% endhighlight %}