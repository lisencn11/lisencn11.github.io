---
layout: post
title: Leetcode Problem 101 Summary
date: 2016-08-15
categories: blog
tags: [study]

---

# 题目

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric

But the following [1,2,2,null,3,null,3] is not

# 我的算法

使用 BFS 遍历每一层，将每一层放入一个 List 中（包括 null ），从两头到中间判断是否对称。

discuss中有 recursive 和 iterative 的 DFS ，也贴在下面。

# 代码

### BFS

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
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int len = queue.size();
            List<TreeNode> list = new LinkedList<>();
            for (int i = 0; i < len; i++) {
                TreeNode iter = queue.poll();
                if (iter != null) {
                    queue.offer(iter.left);
                    queue.offer(iter.right);
                }
                list.add(iter);
            }
            
            for (int i = 0, j = list.size() - 1; i < j; i++, j--) {
                if (list.get(i) == null && list.get(j) == null) continue;
                if (list.get(i) == null || list.get(j) == null) return false;
                if (list.get(i).val != list.get(j).val) return false;
            }
        }
        return true;
    }
}
{% endhighlight %}

### DFS recursive

{% highlight java %}
public class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root==null || isSymmetricHelp(root.left, root.right);
    }
    
    private boolean isSymmetricHelp(TreeNode left, TreeNode right){
        if(left==null || right==null)
            return left==right;
        if(left.val!=right.val)
            return false;
        return isSymmetricHelp(left.left, right.right) && isSymmetricHelp(left.right, right.left);
    }
}
{% endhighlight %}

### DFS iterative

{% highlight java %}
public class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null)  return true;
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode left, right;
        if(root.left!=null){
            if(root.right==null) return false;
            stack.push(root.left);
            stack.push(root.right);
        }
        else if(root.right!=null){
            return false;
        }
            
        while(!stack.empty()){
            if(stack.size()%2!=0)   return false;
            right = stack.pop();
            left = stack.pop();
            if(right.val!=left.val) return false;
            
            if(left.left!=null){
                if(right.right==null)   return false;
                stack.push(left.left);
                stack.push(right.right);
            }
            else if(right.right!=null){
                return false;
            }
                
            if(left.right!=null){
                if(right.left==null)   return false;
                stack.push(left.right);
                stack.push(right.left);
            }
            else if(right.left!=null){
                return false;
            }
        }
        
        return true;
    }
}
{% endhighlight %}