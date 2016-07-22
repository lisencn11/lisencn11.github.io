---
layout: post
title: Segment Tree Summary
date: 2016-07-21
categories: blog
tags: [study]

---

# 线段树

经典应用：  
对于数组 arr[0 ... n-1]，能够：  
1. 计算从下标 l 到下标 r 的和， 0 <= l <= r <= n-1
2. 将数组中指定下标对应的值改为新值 x ，arr[i] = x

以上两种操作：大量且均匀

对于上面这题，无论是每次重新计算加和，还是现存起来加和再每次更新，时间复杂度都不低于O(n)，但是线段树能够达到更新与求和均为O(logn)的时间复杂度。

# 线段树的表示

1. 叶节点是输入的数组
2. 非叶节点是一个 merge 结果，对于此题是从 l 到 r 的和，也可能是别的，比如从 l 到 r 的最小值。

可以用数组表示树，对于 index 为 i 的节点，左子树的 index 为 2 * i + 1，右子树的 index 为 2 * i + 2，父亲节点为 (i - 1) / 2

![segment tree](http://d1gjlxt8vb0knt.cloudfront.net//wp-content/uploads/segment-tree1.png)

# 线段树的构造

{% highlight java %}
int st[];

SegmentTree(int arr[], int n) {
    // Allocate memory for segment tree
    //Height of segment tree
    int x = (int) (Math.ceil(Math.log(n) / Math.log(2)));
 
    //Maximum size of segment tree
    int max_size = 2 * (int) Math.pow(2, x) - 1;
 
    st = new int[max_size]; // Memory allocation
 
    constructSTUtil(arr, 0, n - 1, 0);
}

int constructSTUtil(int arr[], int ss, int se, int si) {
    // If there is one element in array, store it in current node of
    // segment tree and return
    if (ss == se) {
        st[si] = arr[ss];
        return arr[ss];
    }
 
    // If there are more than one elements, then recur for left and
    // right subtrees and store the sum of values in this node
    int mid = getMid(ss, se);
    st[si] = constructSTUtil(arr, ss, mid, si * 2 + 1) +
             constructSTUtil(arr, mid + 1, se, si * 2 + 2);
    return st[si];
}
{% endhighlight %}

首先求出 n 个叶节点对应的树的高度，然后求出最大节点数作为树数组的大小。最后递归求出左子树和右子树的加和， base 条件是叶节点赋值为数组对应值。

# 线段树的查询

{% highlight java %}
int getSum(int n, int qs, int qe) {
    // Check for erroneous input values
    if (qs < 0 || qe > n - 1 || qs > qe) {
        System.out.println("Invalid Input");
        return -1;
    }
    return getSumUtil(0, n - 1, qs, qe, 0);
}

int getSumUtil(int ss, int se, int qs, int qe, int si) {
    // If segment of this node is a part of given range, then return
    // the sum of the segment
    if (qs <= ss && qe >= se)
        return st[si];
 
    // If segment of this node is outside the given range
    if (se < qs || ss > qe)
        return 0;
 
    // If a part of this segment overlaps with the given range
    int mid = getMid(ss, se);
    return getSumUtil(ss, mid, qs, qe, 2 * si + 1) +
            getSumUtil(mid + 1, se, qs, qe, 2 * si + 2);
}
{% endhighlight %}

{% highlight java %}
if (qs <= ss && qe >= se)
    return st[si];
{% endhighlight %}

表示当前节点求和范围属于查询范围之内时，直接返回当前节点值

{% highlight java %}
if (se < qs || ss > qe)
    return 0;
{% endhighlight %}

表示当前节点范围和查询范围无交界时，直接返回0

而对于当前节点范围部分而非全部在查询范围内的情况，则对左右两个子树分别递归求值后加和。

# 线段树的更新

{% highlight java %}
void updateValue(int arr[], int n, int i, int new_val) {
    // Check for erroneous input index
    if (i < 0 || i > n - 1) {
        System.out.println("Invalid Input");
        return;
    }
 
    // Get the difference between new value and old value
    int diff = new_val - arr[i];
 
    // Update the value in array
    arr[i] = new_val;
 
    // Update the values of nodes in segment tree
    updateValueUtil(0, n - 1, i, diff, 0);
}

void updateValueUtil(int ss, int se, int i, int diff, int si) {
    // Base Case: If the input index lies outside the range of 
    // this segment
    if (i < ss || i > se)
        return;
 
    // If the input index is in range of this node, then update the
    // value of the node and its children
    st[si] = st[si] + diff;
    if (se != ss) {
        int mid = getMid(ss, se);
        updateValueUtil(ss, mid, i, diff, 2 * si + 1);
        updateValueUtil(mid + 1, se, i, diff, 2 * si + 2);
    }
}
{% endhighlight %}

同样递归完成各节点的更新，我们更新当前节点后，分别更新左右子节点，对于范围外的节点直接 return 剪枝。