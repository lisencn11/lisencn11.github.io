---
layout: post
title: Leetcode Problem 40 Summary
date: 2016-07-06
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组candidates，一个目标整型target。

**输出**一个List<List<Integer>>，其中的每个List要求加和为target，且每个List不能重复，例如[1, 1, 6]和[1, 6, 1]是重复的。

例如输入[10, 1, 2, 7, 6, 1, 5]和target为8  
输出  
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

# 我的算法

使用DFS算法，回溯实现。coding时遇到的问题：重复List的问题，刚开始是当加和达到target时对List排序判断是否已经加入过，后来发现只需要在一开始对数组进行排序即可。  

# 代码

{% highlight java %}
public class Solution {
    private void getCombination(int[] candidates, int target, int start, List<List<Integer>> result, List<Integer> list) {
        if (target == 0) {
            if (!result.contains(list)) {
                result.add(new ArrayList<Integer>(list));
                return;
            }
        }
        
        for (int i = start; i < candidates.length; i++) {
            if (target - candidates[i] < 0) {
                continue;
            }
            list.add(candidates[i]);
            getCombination(candidates, target - candidates[i], i + 1, result, list);
            list.remove(list.size() - 1);
        }
        return;
    }
    
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        Arrays.sort(candidates);
        getCombination(candidates, target, 0, result, new ArrayList<Integer>());
        return result;
    }
}
{% endhighlight %}

# 优化

当我们发现当前元素与前一个元素相等，且前一个元素没有加入List时，当前元素也无需加入List，对于极端情况如[1, 1, 1, 1, 1, ...]，这个判断条件能大幅减小时间复杂度。

{% highlight java %}
public List<List<Integer>> combinationSum2(int[] cand, int target) {
    Arrays.sort(cand);
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    List<Integer> path = new ArrayList<Integer>();
    dfs_com(cand, 0, target, path, res);
    return res;
}
void dfs_com(int[] cand, int cur, int target, List<Integer> path, List<List<Integer>> res) {
    if (target == 0) {
        res.add(new ArrayList(path));
        return ;
    }
    if (target < 0) return;
    for (int i = cur; i < cand.length; i++){
        if (i > cur && cand[i] == cand[i-1]) continue;
        path.add(path.size(), cand[i]);
        dfs_com(cand, i+1, target - cand[i], path, res);
        path.remove(path.size()-1);
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> ret;
        Arrays.sort(candidates);
        ret = combine(candidates, candidates.length - 1, target);
        return ret;
    }
    
    private List<List<Integer>> combine(int[] candidates, int end, int target) {
        if (target == 0) {
            List<Integer> list = new ArrayList<>();
            List<List<Integer>> ret = new ArrayList<>();
            ret.add(list);
            return ret;
        }
        if (target < 0) return new ArrayList<>();
        if (end < 0) return new ArrayList<>();
        
        int next = end - 1;
        while (next >= 0 && candidates[next] == candidates[end]) next--;
        List<List<Integer>> subList1 = combine(candidates, next, target);
        List<List<Integer>> subList2 = combine(candidates, end - 1, target - candidates[end]);
        
        if (subList2.isEmpty()) return subList1;
        
        for (List<Integer> list : subList2) {
            list.add(candidates[end]);
        }
        
        subList1.addAll(subList2);
        return subList1;
    }
}
{% endhighlight %}