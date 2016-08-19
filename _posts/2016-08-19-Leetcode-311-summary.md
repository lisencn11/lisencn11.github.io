---
layout: post
title: Leetcode Problem 311 Summary
date: 2016-08-19
categories: blog
tags: [study]

---

# 题目

Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

![problem311](lisencn11.github.io/img/problem311.png)

# 我的算法

稀疏矩阵乘法，我的想法是先对 A 矩阵和 B 矩阵进行处理，产生 HashMap<Integer, List<Integer>> 其中 key 对于 A 是行号，对于 B 是列号。List 中存储着 A 和 B 对应行列的非 0 的 index。这样在运算时可以避免对全 0 行列的计算以及跳过占大部分空间的 0。

# 代码

{% highlight java %}
public class Solution {
    public int[][] multiply(int[][] A, int[][] B) {
        Map<Integer, List<Integer>> rowA = new HashMap<>();
        Map<Integer, List<Integer>> colB = new HashMap<>();
        
        for (int i = 0; i < A.length; i++) {
            List<Integer> list = new ArrayList<>();
            for (int j = 0; j < A[0].length; j++) {
                if (A[i][j] != 0) {
                    list.add(j);
                }
            }
            if (list.size() > 0) rowA.put(i, list);
        }
        
        for (int j = 0; j < B[0].length; j++) {
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < B.length; i++) {
                if (B[i][j] != 0) {
                    list.add(i);
                }
            }
            if (list.size() > 0) colB.put(j, list);
        }
        
        int row = A.length;
        int col = B[0].length;
        int[][] res = new int[row][col];
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                res[i][j] = 0;
            }
        }
        for (int i = 0; i < row; i++) {
            if (!rowA.containsKey(i)) continue;
            for (int j = 0; j < col; j++) {
                if (!colB.containsKey(j)) continue;
                List<Integer> a = rowA.get(i);
                List<Integer> b = colB.get(j);
                int p1 = 0;
                int p2 = 0;
                int sum = 0;
                while (p1 < a.size() && p2 < b.size()) {
                    if (a.get(p1) < b.get(p2)) p1++;
                    else if (a.get(p1) > b.get(p2)) p2++;
                    else {
                        sum += (A[i][a.get(p1)] * B[b.get(p2)][j]);
                        p1++;
                        p2++;
                    }
                }
                res[i][j] = sum;
            }
        }
        
        return res;
    }
}
{% endhighlight %}

# discuss

使用HashMap拖慢了速度，discuss中有仅仅使用List就完成计算的，而且我觉得计算逻辑比起我的更加简单。

{% highlight java %}
public int[][] multiply(int[][] A, int[][] B) {
    int m = A.length, n = A[0].length, nB = B[0].length;
    int[][] result = new int[m][nB];

    List[] indexA = new List[m];
    for(int i = 0; i < m; i++) {
        List<Integer> numsA = new ArrayList<>();
        for(int j = 0; j < n; j++) {
            if(A[i][j] != 0){
                numsA.add(j); 
                numsA.add(A[i][j]);
            }
        }
        indexA[i] = numsA;
    }

    for(int i = 0; i < m; i++) {
        List<Integer> numsA = indexA[i];
        for(int p = 0; p < numsA.size() - 1; p += 2) {
            int colA = numsA.get(p);
            int valA = numsA.get(p + 1);
            for(int j = 0; j < nB; j ++) {
                int valB = B[colA][j];
                result[i][j] += valA * valB;
            }
        }
    }

    return result;   
}
{% endhighlight %}