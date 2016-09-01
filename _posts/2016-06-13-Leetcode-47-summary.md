---
layout: post
title: Leetcode Problem 47 Summary
date: 2016-06-13
categories: blog
tags: [study]

---

# 题目

**输入**一个整型数组，其中可以包含重复元素。

**输出**一个List<List>，其中包含整型数组元素的不同顺序组合，每种顺序组合是一个List<Integer>。


# 我的算法

**递归遍历算法**

使用递归，求出从第*i+1*个元素到最后一个元素的所有可能顺序，然后对于每一个可能的*List*，将元素插入到其每一个位置，使用*HashSet<List>*避免重复的顺序组合。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Set<List<Integer>> set = permuteHelper(nums, 0);
        return new ArrayList<List<Integer>>(set);
    }
    
    public Set<List<Integer>> permuteHelper(int[] nums, int pos) {
        Set<List<Integer>> res = null;
        Set<List<Integer>> ret = null;
        List<Integer> list = null;

        if (pos == (nums.length - 1)) {
            list = new ArrayList<Integer>();
            ret = new HashSet<List<Integer>>();
            list.add(nums[pos]);
            ret.add(list);
            return ret;
        }
        
        res = permuteHelper(nums, pos + 1);
        ret = new HashSet<List<Integer>>();
        for (List<Integer> iter : res) {
            for (int i = 0; i < (iter.size() + 1); i++) {
                list = new ArrayList<Integer>(iter);
                list.add(i, nums[pos]);
                ret.add(list);
            }
        }
        
        return ret;
    }
}
{% endhighlight %}

# 高效改良算法

前一个算法的缺陷是对于有大量重复元素的输入效率极低，例如极端情况输入[1,1,1,...,1]，算法复杂度过高。

前一个递归思路是先将后面的所有元素进行排列，再将第一个元素向其中插入。

改良算法的递归思路：从排好序的数组中选出一个未当过首元素的整型作为首元素，然后将剩下的元素进行排列。

# 代码

{% highlight java %}
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        Queue<List<Integer>> queue = new LinkedList<>();
        List<Integer> list = new ArrayList<>();
        permute(queue, nums, nums.length - 1);
        List<List<Integer>> ret = new ArrayList<>();
        while (!queue.isEmpty()) ret.add(queue.poll());
        return ret;
    }
    
    private void permute(Queue<List<Integer>> queue, int[] nums, int end) {
        if (end == 0) {
            List<Integer> list = new ArrayList<>();
            list.add(nums[0]);
            queue.offer(list);
            return;
        }
        
        permute(queue, nums, end - 1);
        int len = queue.size();
        int value = nums[end];
        for (int i = 0; i < len; i++) {
            List<Integer> pre = queue.poll();
            for (int j = 0; j < pre.size() + 1 ; j++) {
                if (j == pre.size()) {
                    pre.add(value);
                    List<Integer> temp = new ArrayList<>(pre);
                    queue.offer(temp);
                    pre.remove(pre.size() - 1);
                    break;
                }
                if (pre.get(j) == value) {
                    pre.add(j, value);
                    List<Integer> temp = new ArrayList<>(pre);
                    queue.offer(temp);
                    pre.remove(j);
                    break;
                }
                pre.add(j, value);
                List<Integer> temp = new ArrayList<>(pre);
                queue.offer(temp);
                pre.remove(j);
            }
        }
    }
}
{% endhighlight %}