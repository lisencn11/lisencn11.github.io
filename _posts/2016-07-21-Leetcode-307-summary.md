---
layout: post
title: Leetcode Problem 307 Summary
date: 2016-07-21
categories: blog
tags: [study]

---

# 题目

### 面向对象编程

**输入**存储一个整型数组，完成以下两个功能。

1. update(int i, int cal) 更新序号 i 的元素的值
2. sumRange(int i, int j) 计算从 nums[i] 到 nums[j] 的加和

update 和 sumRange 操作大量均匀分布

# 我的算法

有两种数据结构支持快捷的更改和求和操作

1. 线段树
2. 树状数组

由于这两种数据结构都没有接触过，所以单写两篇博客学习它们，这里暂时略过。

# 代码

### 线段树

{% highlight java %}
public class NumArray {
    public long[] st = null;
    public int[] nums = null;

    public NumArray(int[] nums) {
        this.nums = nums;
        if (nums == null | nums.length == 0) return;
        int height = (int)Math.ceil((Math.log((double)nums.length) / Math.log(2)));
        int max_size = 2 * (int)Math.pow(2, height) - 1;
        st = new long[max_size];
        
        constructST(0, nums.length - 1, 0);
    }
    
    public long constructST(int start, int end, int iter) {
        if (start == end) {
            st[iter] = nums[start];
            return st[iter];
        }
        
        int mid = start + (end - start) / 2;
        st[iter] = constructST(start, mid, 2 * iter + 1)
                    + constructST(mid + 1, end, 2 * iter + 2);
                    
        return st[iter];
    }

    void update(int i, int val) {
        if (nums == null || nums.length == 0) return;
        int diff = val - nums[i];
        nums[i] = val;
        updateHelper(0, nums.length - 1, i, diff, 0);
    }
    
    public void updateHelper(int start, int end, int i, int diff, int iter) {
        if (i < start || i > end) return;
        
        st[iter] += diff;
        if (start != end) {
            int mid = start + (end - start) / 2;
            updateHelper(start, mid, i, diff, 2 * iter + 1);
            updateHelper(mid + 1, end, i, diff, 2 * iter + 2);
        }
    }

    public int sumRange(int i, int j) {
        if (nums == null || nums.length == 0) return 0;
        return (int)sumHelper(i, j, 0, nums.length - 1, 0);
    }
    
    public long sumHelper(int sumStart, int sumEnd, int start, int end, int iter) {
        if (sumStart <= start && sumEnd >= end) return st[iter];
        
        if (sumStart > end || sumEnd < start) return 0;
        
        int mid = start + (end - start) / 2;
        
        return sumHelper(sumStart, sumEnd, start, mid, 2 * iter + 1)
                + sumHelper(sumStart, sumEnd, mid + 1, end, 2 * iter + 2);
    }
}


// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.update(1, 10);
// numArray.sumRange(1, 2);
{% endhighlight %}

### 树状数组

{% highlight java %}
public class NumArray {
	/**
	 * Binary Indexed Trees (BIT or Fenwick tree):
	 * https://www.topcoder.com/community/data-science/data-science-
	 * tutorials/binary-indexed-trees/
	 * 
	 * Example: given an array a[0]...a[7], we use a array BIT[9] to
	 * represent a tree, where index [2] is the parent of [1] and [3], [6]
	 * is the parent of [5] and [7], [4] is the parent of [2] and [6], and
	 * [8] is the parent of [4]. I.e.,
	 * 
	 * BIT[] as a binary tree:
	 *            ______________*
	 *            ______*
	 *            __*     __*
	 *            *   *   *   *
	 * indices: 0 1 2 3 4 5 6 7 8
	 * 
	 * BIT[i] = ([i] is a left child) ? the partial sum from its left most
	 * descendant to itself : the partial sum from its parent (exclusive) to
	 * itself. (check the range of "__").
	 * 
	 * Eg. BIT[1]=a[0], BIT[2]=a[1]+BIT[1]=a[1]+a[0], BIT[3]=a[2],
	 * BIT[4]=a[3]+BIT[3]+BIT[2]=a[3]+a[2]+a[1]+a[0],
	 * BIT[6]=a[5]+BIT[5]=a[5]+a[4],
	 * BIT[8]=a[7]+BIT[7]+BIT[6]+BIT[4]=a[7]+a[6]+...+a[0], ...
	 * 
	 * Thus, to update a[1]=BIT[2], we shall update BIT[2], BIT[4], BIT[8],
	 * i.e., for current [i], the next update [j] is j=i+(i&-i) //double the
	 * last 1-bit from [i].
	 * 
	 * Similarly, to get the partial sum up to a[6]=BIT[7], we shall get the
	 * sum of BIT[7], BIT[6], BIT[4], i.e., for current [i], the next
	 * summand [j] is j=i-(i&-i) // delete the last 1-bit from [i].
	 * 
	 * To obtain the original value of a[7] (corresponding to index [8] of
	 * BIT), we have to subtract BIT[7], BIT[6], BIT[4] from BIT[8], i.e.,
	 * starting from [idx-1], for current [i], the next subtrahend [j] is
	 * j=i-(i&-i), up to j==idx-(idx&-idx) exclusive. (However, a quicker
	 * way but using extra space is to store the original array.)
	 */

	int[] nums;
	int[] BIT;
	int n;

	public NumArray(int[] nums) {
		this.nums = nums;

		n = nums.length;
		BIT = new int[n + 1];
		for (int i = 0; i < n; i++)
			init(i, nums[i]);
	}

	public void init(int i, int val) {
		i++;
		while (i <= n) {
			BIT[i] += val;
			i += (i & -i);
		}
	}

	void update(int i, int val) {
		int diff = val - nums[i];
		nums[i] = val;
		init(i, diff);
	}

	public int getSum(int i) {
		int sum = 0;
		i++;
		while (i > 0) {
			sum += BIT[i];
			i -= (i & -i);
		}
		return sum;
	}

	public int sumRange(int i, int j) {
		return getSum(j) - getSum(i - 1);
	}
}

// Your NumArray object will be instantiated and called as such:
// NumArray numArray = new NumArray(nums);
// numArray.sumRange(0, 1);
// numArray.update(1, 10);
// numArray.sumRange(1, 2);
{% endhighlight %}