---
layout: post
title: Leetcode Problem 106 Summary
date: 2016-04-25
categories: blog
tags: [study]

---

# 题目

给出二叉树的中序和后序遍历，构建一棵二叉树


# 算法

从后序遍历的最后一个元素可以得到当前二叉树的根节点，然后在当前二叉树的中序遍历中找到该节点的index，从而可以划分出左子树和右子树，对左子树和右子树用相同算法，即回溯算法。

# 代码

```java

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
    
    private TreeNode recursiveBuildTree(int[] inorder, int[] postorder) {
        if (inorder.length == 1) {
            return new TreeNode(inorder[0]);
        } else if (inorder.length == 0) {
            return null;
        }
        
        int len = postorder.length;
        int value = postorder[len - 1];
        TreeNode root = new TreeNode(value);
        int index = 0;
        TreeNode rootLeft = null;
        TreeNode rootRight = null;
        
        for (int i = 0; i < len; i++) {
            if (value == inorder[i]) {
                index = i;
                break;
            }
        }
        
        if (index > 0) {
            rootLeft = recursiveBuildTree(Arrays.copyOfRange(inorder, 0, index), 
                            Arrays.copyOfRange(postorder, 0, index));
        }
        if (index < (len - 1)) {
            rootRight = recursiveBuildTree(Arrays.copyOfRange(inorder, index + 1, len), 
                            Arrays.copyOfRange(postorder, index, len - 1));
        }
                            
        root.left = rootLeft;
        root.right = rootRight;
        return root;
    }
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder == null || inorder.length == 0) {
            return null;
        }
        
        TreeNode root = recursiveBuildTree(inorder, postorder);
        return root;
    }
}

```

# 更进一步

我的程序运行结果很慢，Discussion中得票最高的答案使用相同的算法，但是使用HashMap在inorder中查找根节点，结果很快，于是改用HashMap后的代码如下，这里注意，因为HashMap存储的是inorder全局的index，而之前循环查找的是子树array的index，所以要对传参进行修改。

```java

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
    
    private TreeNode recursiveBuildTree(int[] inorder, int[] postorder, Map<Integer, Integer> map, int is, int ie, int ps, int pe) {
        if (is == ie) {
            return new TreeNode(inorder[ie]);
        } else if (is > ie || ps > pe) {
            return null;
        }
        
        int value = postorder[pe];
        TreeNode root = new TreeNode(value);
        int index = 0;
        TreeNode rootLeft = null;
        TreeNode rootRight = null;
        
        index = map.get(new Integer(value));
        
        if (index > is) {
            rootLeft = recursiveBuildTree(inorder, postorder, map, is, index - 1, ps, ps + index - is - 1);
        }
        if (index < ie) {
            rootRight = recursiveBuildTree(inorder, postorder, map, index + 1, ie, ps + index - is, pe - 1);
        }
                            
        root.left = rootLeft;
        root.right = rootRight;
        return root;
    }
    
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder == null || inorder.length == 0) {
            return null;
        }
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < inorder.length; i++) {
            map.put(new Integer(inorder[i]), new Integer(i));
        }
        
        TreeNode root = recursiveBuildTree(inorder, postorder, map, 0, inorder.length - 1, 0, postorder.length - 1);
        return root;
    }
}


```

