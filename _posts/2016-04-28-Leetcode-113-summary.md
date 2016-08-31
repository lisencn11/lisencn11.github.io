---
layout: post
title: Leetcode Problem 113 Summary
date: 2016-04-28
categories: blog
tags: [study]

---

# 题目

输入一棵二叉树和一个sum，输出从叶子节点到根节点满足条件的所有路径，条件是路径和等于sum。


# 算法

使用回溯算法，分别算出左子树和右子树中满足条件的路径，然后将根节点加到这些路径中。base case分三种，一种是root为null，则返回一个空的List<List<Integer>>，一种是满足条件的根节点，返回一个包含根节点的路径List<List<Integer>>，最后是不满足条件的根节点，返回一个空的List<List<Integer>>。

# 代码

{% highlight java %}
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    
    private static List<List<Integer>> pathSumRec(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null) {
            return result;
        }
        if (root.left == null && root.right == null) {
            if (sum == root.val) {
                List<Integer> list = new ArrayList<Integer>();
                list.add(new Integer(root.val));
                result.add(list);
                return result;
            } else {
                return result;
            }
        }
        
        List<List<Integer>> left = pathSumRec(root.left, sum - root.val);
        List<List<Integer>> right = pathSumRec(root.right, sum - root.val);
        
        left.addAll(right);
        result = left;
        
        for (int i = 0; i < result.size(); i++) {
            result.get(i).add(0, new Integer(root.val));
        }
        return result;
    }
    
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if (root == null) {
            return new ArrayList<List<Integer>>();
        }
        
        List<List<Integer>> result = pathSumRec(root, sum);
        return result;
    }
}
{% endhighlight %}

# 二刷

{% highlight java %}
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        dfs(ret, list, root, sum);
        return ret;
    }
    
    private void dfs(List<List<Integer>> ret, List<Integer> list, TreeNode root, int sum) {
        if (root == null) return;
        if (root.left == null && root.right == null) {
            if (root.val == sum) {
                list.add(root.val);
                ret.add(new ArrayList<>(list));
                list.remove(list.size() - 1);
            }
            return;
        }
        
        int remain = sum - root.val;
        list.add(root.val);
        if (root.left != null) dfs(ret, list, root.left, remain);
        if (root.right != null) dfs(ret, list, root.right, remain);
        list.remove(list.size() - 1);
    }
}
{% endhighlight %}