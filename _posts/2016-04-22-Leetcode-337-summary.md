---
layout: post
title: Leetcode Problem 337 Summary
date: 2016-04-22
categories: blog
tags: [study]

---

# 题目

有一个小区房屋结构是一个二叉树，每个房子是其中一个节点，节点值为房子里的现金。现在有一个小偷，如果偷了相邻的两栋房子警报就会响，请为小偷规划如何不触发警报能偷的钱最多。

例如如下二叉树：

* 第一层：3
* 第二层：4，100
* 第三层：101，2，null，1

最大可偷为101+2+100=203


# 算法

首先可以明显看出这题是二叉树与动态规划的结合，关键是如何应用动态规划，以下是我的算法。

对于一个节点node，以这个节点为根节点的最大偷盗金钱分为两种可能，一种是左子树和右子树的加和，另一种是所有__子树的子树__的加和，加上根节点node。递推方程可写为sum(i) = max{sum(i+1), sum(i+2) + money(i)}。同时，因为当前节点的最大值依赖于左子树和右子树，所以我们除了动态规划还同时需要对二叉树进行后序深度优先搜索(postorder traversal)，这里我使用递归进行后序遍历，子树的结果存放在新的TreeNode中。

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
    
    private static TreeNode robRec(TreeNode root) {
        if (root == null) {
            return new TreeNode(0);
        }
        int max = 0;
        int sumA = 0;
        int sumB = 0;
        TreeNode lChild = robRec(root.left);
        TreeNode rChild = robRec(root.right);
                        
        if (lChild.left != null) {
            sumA += lChild.left.val;
        }
        if (lChild.right != null) {
            sumA += lChild.right.val;
        }
        if (rChild.left != null) {
            sumA += rChild.left.val;
        }
        if (rChild.right != null) {
            sumA += rChild.right.val;
        }
        sumB = lChild.val + rChild.val;
        max = Math.max(sumB, sumA + root.val);
        TreeNode node = new TreeNode(max);
        node.left = lChild;
        node.right = rChild;
        return node;
    }
    
    public int rob(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        TreeNode maxMoney = robRec(root);
        return maxMoney.val;
    }
}
{% endhighlight %}

# 进一步

在discussion中看到得票数最高的答案思路和我的相同，但是存储存储子树数据的方式使用了int[]，而且无须判断子树的子树是否存在，代码更加简洁易懂。

{% highlight java %}
public int rob(TreeNode root) {
    int[] res = robSub(root);
    return Math.max(res[0], res[1]);
}

private int[] robSub(TreeNode root) {
    if (root == null) {
        return new int[2];
    }

    int[] left = robSub(root.left);
    int[] right = robSub(root.right);

    int[] res = new int[2];
    res[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    res[1] = root.val + left[0] + right[0];

    return res;
}
{% endhighlight %}

# 二刷

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

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
    Map<TreeNode, Integer> map = new HashMap<>();
    public int rob(TreeNode root) {
        if (root == null) return 0;
        if (map.containsKey(root)) return map.get(root);
        
        int sum1 = root.val;
        if (root.left != null) sum1 += rob(root.left.left) + rob(root.left.right);
        if (root.right != null) sum1 += rob(root.right.left) + rob(root.right.right);
        
        int sum2 = rob(root.left) + rob(root.right);
        sum1 = sum1 > sum2 ? sum1 : sum2;
        map.put(root, sum1);
        return sum1;
    }
}
{% endhighlight %}