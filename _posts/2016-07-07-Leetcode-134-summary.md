---
layout: post
title: Leetcode Problem 134 Summary
date: 2016-07-07
categories: blog
tags: [study]

---

# 题目

N个加油站围成环形，两个数组：gas[i]表示能够在第i个加油站加多少油，cost[i]表示需要消耗多少油从第i个加油站到第i+1个加油站。

**输入**gas[]和cost[]。

**输出**如果一辆车能够绕一圈的话输出起点的index，如果不能的话输出-1。

# 我的算法

通过gas和cost两个数组，我们可以计算出通过每个加油站汽车油箱的改变量gasLeft，有正有负。

算法：  
1. 从头到尾扫描，如果gasLeft为负则跳过，如果gasLeft为正则以当前加油站为起点进行模拟。  
2. 模拟向前直到：从起点到某一加油站的累积邮箱变化为负表示不能开到；开过了一圈表示当前起点符合要求，返回index。
3. 如果模拟到某个加油站不符合要求，那么我们把起点改变到这个加油站的下一个加油站。因为模拟过程中的任何加油站都不符合要求。
4. 模拟到某个加油站发现此加油站已经在之前模拟过，或者起点超过最后一个加油站则表示没找到符合要求的起点。

# 代码

{% highlight java %}
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int[] gasLeft = new int[gas.length];
        int start = 0;;
        boolean canComplete = false;

        for (int i = 0; i < gas.length; i++) {
            gasLeft[i] = gas[i] - cost[i];
        }
        
        while (start < gas.length && !canComplete) {
            if (gasLeft[start] < 0) {
                start++;
                continue;
            }
            
            int gasSum = 0;
            canComplete = true;
            for (int i = 0; i < gas.length; i++) {
                int index = 0;
                if ((start + i) >=gas.length) {
                    index = start + i - gas.length;
                } else {
                    index = start + i;
                }
                gasSum += gasLeft[index];
                if (gasSum < 0) {
                    if (start > index) {
                        return -1;
                    }
                    start = index + 1;
                    canComplete = false;
                    break;
                }
            }
        }
        if (canComplete == true) {
            return start;
        } else {
            return -1;
        }
    }
}
{% endhighlight %}

# 改进

该算法复杂度无需改进，但是其中的逻辑判断略显复杂，在面试过程中很容易出错，故对代码进行改进。

* 如果汽车从A能开到B-1，但不能开到B，则从A和B间任何一点都不能开到B。
* 如果所有加油站的累积邮箱改变量为正，则一定存在一个加油站能作为起点。

{% highlight java %}
public class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int start = 0, total = 0, tank = 0;
        
        for (int i = 0; i < gas.length; i++) {
            if ((tank += gas[i] - cost[i]) < 0) {
                start = i + 1;
                total += tank;
                tank = 0;
            }
        }
        
        return (total + tank < 0) ? -1 : start;
    }
}
{% endhighlight %}