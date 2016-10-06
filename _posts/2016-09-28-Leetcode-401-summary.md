---
layout: post
title: Leetcode Problem 401 Summary
date: 2016-09-28
categories: blog
tags: [study]

---

# 题目

A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).

Each LED represents a zero or one, with the least significant bit on the right.

![](https://lisencn11.github.io/img/problem401.png)

For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

Example:

Input: n = 1  
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]

Note:

* The order of output does not matter.
* The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
* The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

# 我的算法

backtrack，num 位表示的时间可由 num - 1 位表示的时间求得。

为了避免重复结果，对于 num - 1 位表示的时间，只在第一位 1 之前加 1。

# 代码

{% highlight java %}
public class Solution {
    public List<String> readBinaryWatch(int num) {
        List<String> result = new ArrayList<>();
        if (num == 0) {
            result.add("0:00");
            return result;
        }
        List<String> pre = readBinaryWatch(num - 1);
        for (String s : pre) {
            String[] times = s.split(":");
            int hour = Integer.parseInt(times[0]);
            int min = Integer.parseInt(times[1]);
            if (hour == 0) {
                for (int i = 5; i >= 0; i--) {
                    if ((min & (1 << i)) != 0) break;
                    int newMin = min + (1 << i);
                    if (newMin >= 60) continue;
                    if (newMin < 10) result.add(times[0] + ":0" + newMin);
                    else result.add(times[0] + ":" + newMin);
                }
            }
            for (int i = 3; i >= 0; i--) {
                if ((hour & (1 << i)) != 0) break;
                int newHour = hour + (1 << i);
                if (newHour > 11) continue;
                result.add(newHour + ":" + times[1]);
            }
        }
        return result;
    }
}
{% endhighlight %}