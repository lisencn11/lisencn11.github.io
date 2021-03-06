---
layout: post
title: Leetcode Problem 95 Summary
date: 2016-03-23
categories: blog
tags: [study]

---

# 题目描述

输入一个n，输出所有结构不同的二叉搜索树，树的结点value为1到n。

# 思路

### 动态规划

计算数组result[n+1]，result[i]是一个List，存储的是结点数为i的所有二叉搜索树结果，如：result[0]记录的是0个TreeNode时，树的不同情况，当然为null。

### 初始状态

result[0] = null list

### 状态转移方程

计算result[len]：从i=0遍历到i=len-1，i表示TreeNode数为i的左子树的结构，则i+1为根结点i+2到len为右子树。
对于遍历的每个result，依次取出其中的TreeNode连接到根结点上，这里要注意，我们的右子树要取i+1之后的值，但是右子树的结构是result[len-i-1]，即剩下的长度的结构，所以需要一个clone函数利用result[len-i-1]来构造右子树。

# Java代码

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
    public static List<TreeNode> generateTrees(int n) {
        List<TreeNode>[] result = new List[n+1];
        result[0] = new ArrayList<TreeNode>();
        result[0].add(null);

        for(int len = 1; len <= n; len++){
            result[len] = new ArrayList<TreeNode>();
            for(int j=0; j<len; j++){
                for(TreeNode nodeL : result[j]){
                    for(TreeNode nodeR : result[len-j-1]){
                        TreeNode node = new TreeNode(j+1);
                        node.left = nodeL;
                        node.right = clone(nodeR, j+1);
                        result[len].add(node);
                    }
                }
            }
        }
        return result[n];
    }

    private static TreeNode clone(TreeNode n, int offset){
        if(n == null)
            return null;
        TreeNode node = new TreeNode(n.val + offset);
        node.left = clone(n.left, offset);
        node.right = clone(n.right, offset);
        return node;
    }
}
{% endhighlight %}

# 二刷

二刷的思路是 backtrack 求出 n - 1 个结点的所有可能结果，然后将第 n 个结点依次插入到每一层中。

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
    public List<TreeNode> generateTrees(int n) {
        List<TreeNode> ret = new ArrayList<>();
        TreeNode node = new TreeNode(n);
        
        if (n == 0) return new ArrayList<>();
        if (n == 1) {
            ret.add(node);
            return ret;
        }
        
        List<TreeNode> pre = generateTrees(n - 1);
        for (int i = 0; i < pre.size(); i++) {
            TreeNode root = pre.get(i);
            TreeNode temp = clone(root);
            node = new TreeNode(n);
            node.left = temp;
            ret.add(node);
            int height = 0;
            boolean success = true;
            while (success) {
                temp = clone(root);
                node = new TreeNode(n);
                success = insert(temp, node, height);
                if (success) ret.add(temp);
                height++;
            }
        }
        return ret;
    }
    
    private TreeNode clone(TreeNode root) {
        if (root == null) return null;
        
        TreeNode node = new TreeNode(root.val);
        TreeNode left = clone(root.left);
        TreeNode right = clone(root.right);
        node.left = left;
        node.right = right;
        return node;
    }
    
    private boolean insert(TreeNode root, TreeNode node, int height) {
        if (root == null) return false;
        if (height == 0) {
            TreeNode child = root.right;
            node.left = child;
            root.right = node;
            return true;
        }
        
        return insert(root.right, node, height - 1);
    }
}
{% endhighlight %}