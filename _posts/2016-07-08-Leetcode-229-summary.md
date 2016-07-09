---
layout: post
title: Leetcode Problem 229 Summary
date: 2016-07-08
categories: blog
tags: [study]

---

# 题目

**输入**一个长度为n的整型数组。

**输出**找到所有出现次数大于⌊ n / 3 ⌋的元素。时间复杂度为O(n)，空间复杂度为O(1)。

# 我的算法

多数投票算法，两个int型num变量纪录当前候选者，两个counter纪录当前对应元素次数，从前向后扫描一遍，如果等于两个候选者之一，则对应counter加一；如果均不等于，则看是否有counter已为0，如果有则替换候选者并置counter为1；如果也没有，则两个counter同时减1。

证明：  
如果存在次数大于n / 3的元素，那么它的counter至少加（n / 3） + 1次，而两个counter同时减1的操作则不到(n / 3)次，因为另一个counter不能一直减，至少要加1次再减1次，所以如果存在次数大于n / 3的元素，那么最后它一定会是两个候选者之一，接下来我们再遍历一次数组对两个候选者进行计数即可知道是否存在次数大于n / 3的元素。

# 代码

{% highlight java %}
public class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> ret = new ArrayList<Integer>();
        if (nums == null || nums.length < 1) {
            return ret;
        }
        
        int num1 = nums[0];
        int num2 = 0;
        int count1 = 1;
        int count2 = 0;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            } else if (count1 == 0) {
                num1 = nums[i];
                count1 = 1;
            } else if (count2 == 0) {
                num2 = nums[i];
                count2 = 1;
            } else {
                count1--;
                count2--;
            }
        }
        count1 = 0;
        count2 = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            }
        }
        if (count1 > (nums.length / 3)) {
            ret.add(num1);
        }
        if (count2 > (nums.length / 3)) {
            ret.add(num2);
        }
        return ret;
    }
}
{% endhighlight %}